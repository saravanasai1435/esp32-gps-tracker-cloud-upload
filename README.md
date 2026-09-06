# ESP32_GPS_Tracker_Cloud_Upload

## Purpose
ESP32 GPS tracker with offline data storage and automatic cloud upload when WiFi is available. Reads NMEA sentences from GPS module via UART, parses location data, and uploads to cloud server.

## Hardware
- ESP32 DevKit
- GPS Module (UART, 9600 baud): TX→GPIO16, RX→GPIO17
- 2x Status LEDs: Network (GPIO18), GPS (GPIO19)

## Software
- Language: C++ (Arduino)
- Libraries: WiFi, HTTPClient, ArduinoJson, HardwareSerial
- Server: `https://www.circuitdigest.cloud/geolinker`
- API Key: `G3cHfwwN0rgA` (12 chars)
- WiFi: APFIBER_0978 / 0000009055

## Features
- **GPS Parsing**: GPGGA (location, satellites, altitude) + GPRMC (date)
- **Time Conversion**: UTC to IST (+5:30) with date rollover
- **Offline Buffer**: Stores GPS data in vector when WiFi unavailable
- **Auto Reconnect**: WiFi reconnection on disconnect
- **Status LEDs**: Network LED (WiFi), GPS LED (valid fix)
- **Upload Interval**: Every 10 seconds

## Data Structures
```cpp
struct GPSRawData {
  double latitude, longitude, altitude;
  char latitudeDir, longitudeDir;
  int satellites;
  int hours, minutes, seconds;
  int day, month, year;
  String timestamp;
};

struct GPSData {
  double latitude, longitude;
  String timestamp;  // "YYYY-MM-DD HH:MM:SS"
};
```

## JSON Payload Format
```json
{
  "timestamp": ["2025-09-05 10:30:45"],
  "lat": [12.9716],
  "long": [77.5946]
}
```

## Pinout
- GPS RX: GPIO 16 (ESP32 UART1 RX)
- GPS TX: GPIO 17 (ESP32 UART1 TX)
- Network LED: GPIO 18
- GPS LED: GPIO 19

## NMEA Parsing
- **GPGGA**: Time, Lat/Lon, Fix quality, Satellites, Altitude
- **GPRMC**: Date, Status, Speed, Course
- Decimal conversion: `degrees + minutes/60`
- Minimum 4 satellites for valid fix

## Status
- **Working**: Complete implementation with offline buffering
- **Confidence**: HIGH

## Related Projects
- Original folder: `esp32tracker` (with debug configs)
- Similar: `ESP32_GPS_TFT_Display` (sketch_jul13a)