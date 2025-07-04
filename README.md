# 🤖 ESW (Embedded Systems Workshop) Repository

A comprehensive IoT project that combines Arduino-based robot control with Flutter mobile application for voice-controlled vehicle movement using Azure Cognitive Services.

## 🎯 Project Overview

This repository contains an **Internet of Things (IoT)** project that demonstrates:
- **Voice-controlled robot car** using Arduino ESP32
- **Multi-language speech recognition** using Azure Cognitive Services
- **Real-time translation** capabilities
- **Cross-platform mobile control** via Flutter application
- **WiFi-based communication** between mobile app and Arduino

## 🏗️ Repository Structure

```
esw_repo/
├── arduino_codes/           # Arduino ESP32 firmware
│   ├── arduino_controller.ino  # Main Arduino sketch
│   └── readme              # (empty) Arduino documentation
└── flutter_code/           # Flutter mobile application
    ├── lib/
    │   └── main.dart       # Main Flutter application
    ├── assets/             # UI direction icons
    │   ├── up.png
    │   ├── down.png
    │   ├── left.png
    │   └── right.png
    ├── pubspec.yaml        # Flutter dependencies
    └── README.md           # Flutter project info
```

## 🚗 Arduino Controller Features

### Hardware Components
- **ESP32 Development Board** - WiFi-enabled microcontroller
- **L298N Motor Driver** - Dual H-bridge motor controller
- **DC Motors (2x)** - For robot movement
- **WiFi Connectivity** - For HTTP communication

### Motor Control System
- **Motor A & B Control**: Independent control of left and right motors
- **PWM Speed Control**: Variable speed control (0-255)
- **Direction Control**: Forward, reverse, left, right, stop

### Communication Protocol
- **HTTP Server**: Runs on port 80
- **REST API Endpoint**: `/send` (POST method)
- **Command Processing**: Text-based commands (FORWARD, REVERSE, LEFT, RIGHT, STOP)
- **Response Codes**: Numeric feedback (1-5) for different actions

### Pin Configuration
```cpp
// Motor A (Left Motor)
ENA (Speed):     Pin 19  // PWM for speed control
IN1 (Direction): Pin 17  // Motor direction control
IN2 (Direction): Pin 5   // Motor direction control

// Motor B (Right Motor)  
ENB (Speed):     Pin 21  // PWM for speed control
IN3 (Direction): Pin 16  // Motor direction control
IN4 (Direction): Pin 4   // Motor direction control
```

### Movement Logic
- **Forward**: Both motors move forward at speed 150
- **Reverse**: Both motors move backward at speed 150
- **Left Turn**: Left motor forward (speed 200), right motor stops, 1-second duration
- **Right Turn**: Right motor forward (speed 200), left motor stops, 1-second duration
- **Stop**: Both motors disabled (speed 0)

## 📱 Flutter Mobile Application Features

### Core Functionality
- **Multi-language Speech Recognition** (English, Hindi, Punjabi, French, German)
- **Real-time Audio Recording** using device microphone
- **Azure Cognitive Services Integration** for speech-to-text
- **Automatic Language Translation** to English
- **Robot Control Commands** via HTTP requests

### Azure Integration
#### Speech-to-Text Service
- **Service**: Azure Cognitive Services Speech API
- **Region**: Central India
- **Supported Languages**: 5 languages with locale codes
- **Audio Format**: 16kHz PCM WAV

#### Translation Service
- **Service**: Azure Translator API
- **Auto-translation**: Non-English languages → English
- **Real-time Processing**: Immediate translation after speech recognition

### User Interface Components
1. **Language Selector**: Dropdown for choosing input language
2. **Voice Recording Button**: Start/stop audio recording
3. **Text Display**: Shows recognized and translated text
4. **Send Command Button**: Transmits command to Arduino
5. **Direction Indicators**: Visual feedback with highlighted arrows
6. **Status Display**: Shows current robot action
7. **Emergency Stop**: Red stop button for immediate halt

### Communication Flow
```
User Speech → Audio Recording → Azure STT → Translation → Arduino Command → Robot Action → UI Feedback
```

## 🔧 Setup and Installation

### Arduino Setup

1. **Hardware Assembly**
   ```
   ESP32 → L298N Motor Driver → DC Motors
   ```

2. **Arduino IDE Configuration**
   - Install ESP32 board support
   - Install required libraries: WiFi, WebServer

3. **Code Configuration**
   ```cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   ```

4. **Upload Firmware**
   - Connect ESP32 via USB
   - Select board: "ESP32 Dev Module"
   - Upload `arduino_controller.ino`

5. **Get IP Address**
   - Monitor Serial output for ESP32 IP address
   - Update Flutter app with this IP

### Flutter App Setup

1. **Prerequisites**
   ```bash
   flutter --version  # Ensure Flutter is installed
   ```

2. **Install Dependencies**
   ```bash
   cd flutter_code
   flutter pub get
   ```

3. **Azure Service Keys**
   ```dart
   // Update in main.dart
   final String subscriptionKey = 'YOUR_AZURE_SPEECH_KEY';
   final String translationKey = 'YOUR_AZURE_TRANSLATOR_KEY';
   final String region = 'centralindia';
   ```

4. **Arduino IP Configuration**
   ```dart
   String url = 'http://YOUR_ARDUINO_IP/send';
   ```

5. **Run Application**
   ```bash
   flutter run
   ```

## 🔑 Azure Services Configuration

### Speech Service Setup
1. Create Azure Cognitive Services Speech resource
2. Get subscription key and region
3. Configure supported languages in Flutter app

### Translator Service Setup
1. Create Azure Translator resource
2. Get subscription key
3. Configure translation endpoints

## 🎮 Usage Instructions

### Voice Control Commands
- **"आगे बढ़ो"** (Hindi) → "move forward" → Robot moves forward
- **"पीछे जाओ"** (Hindi) → "go back" → Robot moves backward  
- **"बाएं मुड़ो"** (Hindi) → "turn left" → Robot turns left
- **"दाएं मुड़ो"** (Hindi) → "turn right" → Robot turns right
- **"रुको"** (Hindi) → "stop" → Robot stops

### Operation Flow
1. **Select Language**: Choose input language from dropdown
2. **Record Command**: Tap microphone button and speak
3. **Process Audio**: App processes speech and shows translation
4. **Send Command**: Tap send button to transmit to robot
5. **Visual Feedback**: Direction arrows highlight based on action
6. **Emergency Stop**: Use red STOP button for immediate halt

## 🛠️ Technical Specifications

### Arduino Technical Details
- **Microcontroller**: ESP32-WROOM-32
- **Operating Voltage**: 3.3V/5V
- **WiFi Standard**: 802.11 b/g/n
- **Motor Driver**: L298N Dual H-Bridge
- **PWM Resolution**: 8-bit (0-255)
- **Communication**: HTTP RESTful API

### Flutter Technical Details
- **Framework**: Flutter 3.5.3+
- **Language**: Dart
- **Audio Recording**: flutter_sound package
- **HTTP Client**: Built-in http package
- **UI Notifications**: fluttertoast package
- **Permissions**: microphone access

### Network Requirements
- **WiFi Network**: Both devices on same network
- **Internet Access**: Required for Azure services
- **Firewall**: Port 80 open for Arduino communication

## 🔐 Security Considerations

- **API Keys**: Store Azure keys securely (environment variables recommended)
- **Network Security**: Use WPA2/WPA3 encrypted WiFi
- **Input Validation**: Arduino validates incoming commands
- **Error Handling**: Comprehensive error handling in both components

## 🎯 Use Cases

### Educational Applications
- **Robotics Learning**: Hands-on IoT and robotics education
- **Language Technology**: Speech recognition and translation demo
- **Embedded Systems**: Arduino programming and WiFi communication

### Practical Applications
- **Voice-controlled Wheelchair**: Accessibility applications
- **Smart Home Automation**: Voice control for IoT devices
- **Industrial Automation**: Voice commands in manufacturing

## 🔮 Future Enhancements

### Hardware Upgrades
- **Camera Integration**: Add ESP32-CAM for video streaming
- **Sensor Array**: Ultrasonic sensors for obstacle avoidance
- **Battery Management**: Rechargeable battery system
- **LED Indicators**: Status indication lights

### Software Improvements
- **Offline Mode**: Local speech recognition capability
- **Custom Commands**: User-defined voice commands
- **Multi-robot Control**: Control multiple robots simultaneously
- **Data Logging**: Movement history and analytics

### Advanced Features
- **Machine Learning**: Custom voice model training
- **Computer Vision**: Object recognition and following
- **Path Planning**: Autonomous navigation algorithms
- **Remote Monitoring**: Cloud-based robot monitoring

## 🐛 Troubleshooting

### Common Issues

#### Arduino Connection Issues
```
Problem: Robot not responding to commands
Solution: 
1. Check WiFi connection status
2. Verify IP address in Flutter app
3. Monitor Arduino Serial output
4. Test with HTTP client (Postman)
```

#### Speech Recognition Problems
```
Problem: Poor speech recognition accuracy
Solution:
1. Ensure good microphone quality
2. Speak clearly and slowly
3. Check internet connectivity
4. Verify Azure service status
```

#### Motor Control Issues
```
Problem: Robot moves erratically
Solution:
1. Check motor driver connections
2. Verify power supply voltage
3. Test individual motor functions
4. Check PWM signal integrity
```

## 📄 API Reference

### Arduino REST API

#### Send Command Endpoint
```http
POST /send
Content-Type: text/plain
Body: COMMAND_STRING

Responses:
- "Code: 1" - Forward command executed
- "Code: 2" - Reverse command executed  
- "Code: 3" - Left turn executed
- "Code: 4" - Right turn executed
- "Code: 0" - Stop command executed
```

#### Supported Commands
- `FORWARD` - Move robot forward
- `REVERSE` - Move robot backward
- `LEFT` - Turn robot left
- `RIGHT` - Turn robot right
- `STOP` - Stop robot movement

## 📚 Dependencies

### Arduino Libraries
```cpp
#include <WiFi.h>        // ESP32 WiFi functionality
#include <WebServer.h>   // HTTP server implementation
```

### Flutter Dependencies
```yaml
dependencies:
  flutter_sound: ^9.16.2      # Audio recording
  http: ^0.13.6               # HTTP client
  fluttertoast: ^8.2.1        # Toast notifications
  permission_handler: ^10.2.0 # Microphone permissions
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

**Built for Embedded Systems Workshop - IoT and Robotics Education** 🎓🤖
