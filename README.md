# AeroSense AQ-1
> Drone-mounted air quality monitor for spatial pollution mapping over Dhaka, Bangladesh.

Built on an ESP32-S3, the AQ-1 logs particulate matter (PM1.0, PM2.5, PM10), temperature, humidity, pressure, and raw CO/NOx readings to a timestamped CSV at 5-second intervals during flight.

---

## Sensors

| Sensor | Interface | Measures |
|---|---|---|
| Plantower PMS7003 | UART (9600 8N1) | PM1.0, PM2.5, PM10 |
| Bosch BME280 | I²C `0x76` | Temperature (°C), relative humidity (%), pressure (hPa) |
| SGX MICS-4514 | ADC | CO, NOx (raw ADC values) |
| PCF8563 | I²C | Real-time clock |
| I2C LCD (16x2) | I²C `0x27` | Live on-device readout |

---

## Wiring

```
ESP32-S3 GPIO 8  (SDA) ──── BME280, PCF8563 RTC, LCD
ESP32-S3 GPIO 9  (SCL) ──── BME280, PCF8563 RTC, LCD
ESP32-S3 GPIO 17 (RX)  ──── PMS7003 TX
ESP32-S3 GPIO 18 (TX)  ──── PMS7003 RX
ESP32-S3 GPIO 4        ──── MICS-4514 NOx out
ESP32-S3 GPIO 5        ──── MICS-4514 RED (CO) out
ESP32-S3 GPIO 3  (CS)  ──── microSD
ESP32-S3 GPIO 1  (MOSI)──── microSD
ESP32-S3 GPIO 2  (MISO)──── microSD
ESP32-S3 GPIO 6  (SCK) ──── microSD
```

> **Note:** GPIO 1 and 3 are shared with USB-Serial on some ESP32-S3 boards — watch for upload/boot issues once the SD card is wired in.

---

## CSV output format

One file per day: `/log_YYYYMMDD.csv`

```
Date,Time,PM1.0,PM2.5,PM10,Temp_C,Humidity_%,Pressure_hPa,CO_raw,NO2_raw
```

A new file is created automatically each day the RTC reports a new date. Rows are appended every 5 seconds while the system is running.

---

## Repository structure

```
FX-AeroSense-AQ1/
├── firmware/
│   └── sensor_setup.ino
├── analysis/               # data analysis pipeline (in development)
├── data/
│   └── sample/
│       └── sample_data.csv
├── hardware/
│   └── wiring_diagram.pdf
├── .gitignore
└── README.md
```

---

## License
MIT © FX AeroSense
