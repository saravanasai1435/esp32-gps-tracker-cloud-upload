# ESP32 GPS Tracker with Cloud Upload

ESP32 GPS tracker with NMEA parsing, offline data buffering, and automatic cloud upload when WiFi is available.

## How the Code Works

1. **GPS Initialization**: UART1 (GPIO16/17) at 9600 baud for GPS module. WiFi connection to AP.

2. **NMEA Parsing**: Continuous reading from GPS serial. Parses GPGGA and GPRMC sentences.

3. **Time Conversion**: UTC to IST (+5:30) with date rollover handling.

4. **Offline Buffer**: std::vector stores data when WiFi disconnected. Auto-flushes on reconnect.

5. **Cloud Upload**: HTTP POST to CircuitDigest cloud every 10 seconds with JSON payload.

6. **Status LEDs**: Network LED (GPIO18) = WiFi status. GPS LED (GPIO19) = valid fix.

## Main Components

- ESP32 DevKit + GPS Module (UART, 9600 baud)
- 2 Status LEDs (Network, GPS)
- CircuitDigest Cloud (geolinker endpoint)

## How to Run

1. Wire GPS: TX→GPIO16, RX→GPIO17, VCC→3.3V, GND→GND
2. Update WiFi credentials in code
3. Flash via Arduino IDE or PlatformIO
4. Monitor serial at 115200 baud