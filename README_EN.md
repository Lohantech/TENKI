[日本語](./README.md) | [English](./README_EN.md)

# Introduction to the TENKI Project

## Project Purpose

Equipped with a Base Station installed inside the home and a Mobile Beacon placed outdoors or indoors, the purpose of the TENKI project is to monitor indoor CO2 levels (air quality)—which are known to have health impacts—as well as indoor and outdoor temperatures. Indoor and outdoor humidity sensors identify damp areas outside or inside rooms, addressing humidity and mold issues in Japan. When placed outdoors, the Mobile Beacon's light sensor detects outdoor brightness levels (such as overcast, night, or clear sunny skies) and allows the system to display current weather conditions and weather forecasts.

## Components Used

### Home Station
* **OLED Display**: 1.5-inch SH1107 (IIC I2C 4-pin 128x128)
* **Board**: Arduino UNO R4 WiFi \[ABX00087\] (with built-in ESP32)
* **Sensor**: 1x Carbon Dioxide (CO2) sensor module SCD40 (up to 2,000 ppm)
* **Wiring**: Male-to-female jumper wires
* **Power Supply**:
  * Anker Zolo USB-C cable (1 m, 240 W, white)
  * Amazon Basics USB-C wall charger Nano GaN (white)

### Mobile Beacon
* **Board**: DUBEUYEW 3-pack ESP32-C3 Mini development board ESP32-C3 Super Mini (Type-C, router-free local 2.4 GHz communication, Wi-Fi + Bluetooth)
* **Sensors**:
  * **Light Sensor**: BH1750
  * **Environmental Sensor**: BME280
* **Wiring**: Female-to-female jumper wires
* **Power Supply**: 3x AA battery holder

## Product Design

* The product must be harmonious, gentle, easy to understand, and simple to use for mothers and their families. The most natural and easy-to-place form factor is a white square frame that can lie flat on furniture or the floor, or be tilted using a kickstand system.
* The plastic used is durable, lightweight, and pleasant-to-the-touch PLA filament.
* When humidity is high or CO2 levels become too high and require ventilation (opening a window), a white flashing alert appears on the screen so the user can regain peace of mind without worrying about their home environment.
* Warning alerts for typhoons, thunderstorms, and heavy rain can also be triggered.
* To blend seamlessly with Japanese homes and apartments—where square and rectangular geometric shapes are widely used—a 1.5-inch monochrome square OLED display was chosen so as not to disrupt the visual harmony of Japanese living spaces.
* Using an OLED screen displays only the necessary information without overstimulation; pixels set to 0 emit no artificial light and remain pure black, preventing eye strain day and night while significantly reducing power consumption.
* The white color of the monochrome interface conveys information with high readability without disrupting the product's harmony or causing excessive visual stimulation.
* For power, a modern global standard USB-C connection is used with a compact, discreet power adapter and cable that stay out of the way.
* The Mobile Beacon is a small white cube that can easily be placed wirelessly outdoors or indoors.
* Because it is battery-powered, it can be placed anywhere very easily without any risk of falling, collecting and sending data to the Base Station harmoniously and effortlessly.
* The Mobile Beacon is waterproof to withstand humidity and outdoor conditions, featuring ventilation holes on one face of the enclosure along with a waterproof gasket (seal).

## Graphical User Interface (UI)

* In keeping with the project's concept, the screen interface is displayed in Japanese. Text is displayed horizontally from left to right, with the date and time positioned at the top of the screen.
* Icons are displayed and change according to the forecasted weather conditions. Alerts appear as white flashes or animations.
* The layout adopts a "Bento Grid" format to structure information clearly for optimal readability, harmonizing with the core project concept so it blends naturally into Japanese apartment and home interiors.

## Code Logic and Forecasting

* Regarding the Mobile Beacon and its battery life, using three 1.5V AA batteries combined with the ESP32's built-in "Deep Sleep" mode allows it to run for several months without worrying about battery levels.
* In this mode, the Mobile Beacon wakes up every 16 minutes to quickly capture data, transmits it to the Home Base Station, and immediately goes back to sleep, consuming little to no power.
* Depending on the transmission distance, each active cycle lasts about 2 seconds in total, followed by 15 minutes and 58 seconds of sleep, achieving a battery life of 7 to 9 months depending on operating conditions.
* To communicate simply and rapidly even over long distances, the system uses 2.4 GHz UDP communication in Access Point (AP) mode (without going through an internet router), sending data directly without acknowledgments.
* By establishing a dedicated local network between the chips using 2.4 GHz Wi-Fi technology, the signal penetrates walls and reaches further, achieving a range of 30 to 60 meters outdoors without obstacles and 10 to 25 meters indoors through walls.
* Because it uses no APIs, internet connection, or router, the product is guaranteed to operate 100% locally.

## Data Structuring

### Data Collected by the Mobile Beacon
The Mobile Beacon collects the following data:

* **Outdoor Temperature (unit: °C)**: Measured by the BME280 sensor.
* **Relative Humidity (unit: %)**: Measured by the BME280 sensor.
* **Atmospheric Pressure (unit: hPa)**: Measured by the BME280 sensor.
* **Ambient Light Level (unit: Lux)**: Measured by the BH1750 sensor.

This data is then transmitted to the Home Station to be processed and presented to the user. Background calculations and data validation (checking thresholds, running calculations for accurate forecasts, and comparing against historical or different datasets) are performed before displaying or analyzing the information.

### Data Measured and Processed by the Home Station
For the interior of the house or apartment, the Home Station's sensors measure the following parameters:

* **CO2 Concentration (unit: ppm)**: Measured by the SCD40 sensor (to monitor air quality and know when to ventilate).
* **Room Temperature (unit: °C)**: Measured by the SCD40 sensor.
* **Indoor Humidity (unit: %)**: Measured by the SCD40 sensor.

Based on this raw data, the Home Station performs background analysis and calculations before communicating results to the user. It verifies data consistency and historical records, processing everything according to its programmed logic. Upon receiving readings from the Mobile Beacon, it cross-checks them against its own measurements to confirm consistency, runs calculations and analysis, and can then issue weather forecasts and alerts.
