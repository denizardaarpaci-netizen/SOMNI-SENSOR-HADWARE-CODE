# SOMNI – Sensor Hardware & Arduino Code

This repository contains the Arduino code developed for **SOMNI**, a screenless fitness tracker prototype. It documents the process of testing individual sensors and combining them into one system using the XIAO ESP32-C6.

The code is organized by development stage, from the first heart-rate experiment to the integrated sensor system.

## 1. Repository Files

| File | Components required | Description |
|---|---|---|
| **10K NTC, LISD3DSH, MAX30102 together** | XIAO ESP32-C6, 10K NTC thermistor, 10KΩ resistor, LIS3DSH, MAX30102 | Integrated version intended to read temperature, movement, heart rate and experimental SpO2 data together. Use this as the main version once the updated NTC code is uploaded. |
| **LIS3DH and MAX30102 together final** | XIAO ESP32-C6, LIS3DSH, MAX30102 | Combines heart-rate monitoring, experimental SpO2 estimation and accelerometer readings. Does not include the thermistor. |
| **LSD3DSH Accelerometer Code** | XIAO ESP32-C6, LIS3DSH | Standalone accelerometer test. Reads X, Y and Z acceleration values and outputs them in g. Useful for checking the accelerometer before integration. |
| **MAX30102 Heart Rate Sensor Final Code** | XIAO ESP32-C6, MAX30102 | Standalone optical sensor test. Calculates an averaged heart rate and an experimental SpO2 estimate. |
| **MAX30103 Heart Rate Sensor Code Prototype 1** | XIAO ESP32-C6, MAX3010x-compatible sensor | Early heart-rate sensor experiment. This is an initial prototype and is less developed than the later MAX30102 code. It is kept as a record of the development process, not as the recommended version. |

## 2. Hardware Connections

The integrated system uses the XIAO ESP32-C6.

### Shared I2C connections

Both the MAX30102 and LIS3DSH communicate through I2C.

| XIAO ESP32-C6 | MAX30102 | LIS3DSH |
|---|---|---|
| GPIO 22 (SDA) | SDA | SDA |
| GPIO 23 (SCL) | SCL | SCL |
| 3.3V | VIN/VCC | VCC |
| GND | GND | GND |

Both sensors share the same SDA and SCL connections. The LIS3DSH is configured at I2C address `0x18`.

### 10K NTC Thermistor

The thermistor uses an analog voltage divider.

| Connection | Component |
|---|---|
| 3.3V | One side of the fixed 10KΩ resistor |
| A0 | Junction between the resistor and NTC |
| GND | Other side of the NTC |

The arrangement is:

`3.3V → 10KΩ resistor → A0 → 10K NTC → GND`

The temperature calculation depends on the thermistor's Beta value and calibration.

## 3. Which Code Should I Use?

**For testing individual components:**

1. Start with the MAX30102 standalone code to test heart-rate readings.
2. Upload the LIS3DSH standalone code to check acceleration readings.
3. Test the NTC thermistor using the integrated version.

**For testing the complete system:**

Use `10K NTC, LISD3DSH, MAX30102 together` after confirming that the updated version containing the thermistor code is uploaded.

The older combined version without the NTC is useful as a fallback if you need to troubleshoot the optical sensor or accelerometer separately.

## 4. Software Requirements

- Arduino IDE
- XIAO ESP32-C6 board support
- SparkFun MAX3010x Sensor Library
- `Wire.h` (included with Arduino)
- `heartRate.h` (provided by the MAX3010x library)

Select the XIAO ESP32-C6 board, choose the correct COM port, upload the sketch and open Serial Monitor at **115200 baud**.

## 5. Development Notes

These sketches represent different stages of SOMNI's hardware development. The early prototype code is preserved to document experimentation and improvements.

The SpO2 calculation is experimental and has not been medically validated. Temperature readings also require calibration. These prototypes are intended for engineering development and testing, not medical diagnosis or clinical monitoring.

---

**Project:** SOMNI – Screenless Fitness Tracker  
**Microcontroller:** Seeed Studio XIAO ESP32-C6  
**Developer:** Deniz Arda Arpacı
