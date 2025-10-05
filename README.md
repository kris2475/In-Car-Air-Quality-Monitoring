# AeroGuard: High-Resolution, Geo-Tagged Data Platform

## 🌟 Introduction: The Polluted Micro-Environment
The air inside a vehicle's cabin often contains pollutant concentrations far exceeding those found outside. Whether due to traffic jams, tunnel transit, or simply driving near industrial areas, this confined space represents a critical micro-environment where real-time exposure can be at its highest.  

However, high-fidelity data correlating these internal air spikes with external factors—like location, time, speed, and surrounding weather—is scarce.  

This is the **"why"** behind **AeroGuard**. It is not designed merely to display current conditions, but to act as a scientific instrument dedicated to robust, mobile data acquisition.  

AeroGuard’s core mission is to overcome this data gap. Built on the **ESP32 platform**, it is engineered to generate a massive, rich, and multivariate dataset that links precise geographic location (GPS) and time with **24 different environmental, atmospheric, and chemical features**. This complex, multi-sensor data is immediately structured for subsequent processing and analysis, serving as foundational input for **Machine Learning (ML) and Deep Learning (DL)** initiatives.  

---

## 🤖 Data for Machine Learning and Deep Learning
The primary output of the AeroGuard system is a vast, correlated dataset, ideal for training advanced ML/DL models aimed at predictive air quality and complex environmental analysis.

### Data Value Proposition
- **Multivariate Feature Set**: Logs 24 distinct, correlated features, providing a granular view of the environment at every point in time.  
- **High-Frequency Temporal Resolution**: 1-second logging captures rapid micro-changes, crucial for time series analysis and dynamic prediction models (e.g., LSTMs).  
- **Geo-Tagged Spatiotemporal Data**: Each datapoint is tagged with GPS coordinates, speed, and heading, enabling models to learn spatial relationships between geography, vehicle dynamics, and pollution concentration.  

### Example Applications
- **Predictive Air Quality Mapping**: Train models to predict pollutant levels (PM2.5, CO) based on location, time, and atmospheric conditions.  
- **Pollutant Source Classification**: Classify pollution sources (e.g., traffic, industrial plumes, wildfires) using PM + gas sensor fusion.  
- **Sensor Fusion & Cross-Calibration**: Correct or calibrate one sensor’s output based on correlated readings from others.  

---

## 🎯 Outcomes of Trained ML/DL Models

### 1. Real-time Predictive Mapping
| Model Application | Description | Data Requirement |
|-------------------|-------------|------------------|
| Micro-Environment Forecasting | Predict pollution level (PM2.5, CO) at streets/tunnels before entering. | Geo-Tagged Spatiotemporal Data |
| Sensor Value Estimation | Impute missing sensor values using correlated features. | Multivariate Feature Set |
| Filter Saturation Prediction | Forecast cabin filter replacement needs based on cumulative PM/gas exposure. | Time-Series Analysis |

### 2. Actionable Insights and Behavioral Change (The *"So What"*)
- **Optimal Route Planning**: Navigation recommends *low-exposure* routes, not just fastest ones.  
- **Intelligent Ventilation Control**: Automatically switch between fresh air and recirculation before pollution spikes.  
- **Exposure Attribution**: Attribute exposure to sources (e.g., highway driving vs. garage entry).  

### 3. Policy and Infrastructure Support (The *"Bigger Picture"*)
- **High-Resolution Pollution Maps**: Dynamic, block-by-block maps revealing localized hot spots.  
- **Infrastructure Effectiveness Analysis**: Measure impact of road layouts, green barriers, ventilation systems.  
- **Traffic Management Optimization**: Use pollution correlations to recommend traffic light adjustments.  

---

## 🚀 System Features
- **Integrated GPS Logging**: Latitude, longitude, altitude, speed, heading.  
- **Comprehensive Environmental Data**: PM1.0, PM2.5, PM10 + temperature, humidity, pressure, gas resistance.  
- **Chemical Sensing**: NO₂, CO, H₂S (via calibrated analog voltage).  
- **Efficient Data Handling**: Buffered logging, batch SD writes every 10 minutes (600 entries).  
- **Mandatory Warmup**: 30 min BME680 stabilization before logging.  

---

## 🛠️ Hardware Requirements (Assumed)

| Component             | Purpose                 | Interface/Pins         | ESP32 Pin(s)          |
|-----------------------|-------------------------|------------------------|-----------------------|
| **SD Card Module**    | Data Storage            | SPI (CS)               | GPIO5                 |
| **GPS Module**        | Location/Time           | UART2                  | GPIO16 (RX), GPIO17 (TX) |
| **PMSA003I Sensor**   | Particulate Matter      | I2C                    | GPIO21 (SDA), GPIO22 (SCL) |
| **BME680 Sensor**     | Temp/Humidity/Pressure/Gas | I2C                 | GPIO21 (SDA), GPIO22 (SCL) |
| **NO₂ Gas Sensor**    | Gas Sensing             | Analog                 | GPIO33 (ADC1_CH5)     |
| **CO Gas Sensor**     | Gas Sensing             | Analog                 | GPIO34 (ADC1_CH6)     |
| **H₂S Gas Sensor**    | Gas Sensing             | Analog                 | GPIO35 (ADC1_CH7)     |
| **SSD1306 OLED**      | Status Display          | I2C                    | GPIO21 (SDA), GPIO22 (SCL) |

---

## 💻 Software and Dependencies
Arduino libraries required:
- `SPI`, `SD` – SD card communication  
- `TinyGPS++` – GPS parsing  
- `HardwareSerial` – ESP32 UART2 support  
- `Wire` – I2C communication  
- `Adafruit_PM25AQI` – PMSA003I interface  
- `Adafruit_BME680` – BME680 interface  
- `Adafruit_GFX`, `Adafruit_SSD1306` – OLED display drivers  
- `driver/adc.h`, `esp_adc_cal.h` – ESP32 ADC calibration  

---

## ⚙️ Operational Procedure
1. **Code Upload**: Upload `AeroGuard_ver4.ino` to ESP32.  
2. **Power On & Warmup**: OLED shows *Warmup* status. Wait 30 minutes for BME680 stabilization.  
3. **Data Logging**: Logging begins automatically, 1 Hz. OLED shows GPS lock, PM2.5, buffer count.  
4. **Data Write**: Every 600 entries (~10 min), data writes to SD (`WRITE COMPLETE` or `SD WRITE FAILED!`).  
5. **Data Retrieval**: Power off, remove SD. Data saved in sequential `.csv` files:
   
/gpslog0.csv, /gpslog1.csv, ...


---

## 📊 Data Format (CSV Header)

timestamp,latitude,longitude,altitude,satellites,speed,heading,hdop,
pm1_standard,pm25_standard,pm10_standard,
particles_03um,particles_05um,particles_10um,particles_25um,
particles_50um,particles_100um,
bme_temperature_c,bme_humidity_percent,bme_pressure_hpa,bme_gas_resistance_ohms,
no2_voltage_v,co_voltage_v,h2s_voltage_v


---

## 🏁 Summary
The ultimate value of **AeroGuard** is turning raw, high-frequency measurements into **intelligence** that enables:  
- Healthier individual decision-making  
- Smarter urban planning  
- Better environmental policy  
 
