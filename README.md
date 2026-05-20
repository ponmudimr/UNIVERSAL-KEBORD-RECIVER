# KeyRescue – RF Keyboard to Bluetooth Adapter

## Project Overview

KeyRescue is an embedded systems project that aims to revive old wireless keyboards whose USB dongles are lost. Most wireless keyboards still work internally but become useless because their proprietary RF USB receiver is missing.

This project uses an ESP32 and NRF24L01 module to:

* Receive RF packets from wireless keyboards
* Decode keyboard input
* Convert the input into Bluetooth HID
* Allow laptops, tablets, and mobiles to use the keyboard through Bluetooth

---

# Problem Statement

Many wireless keyboards use:

* Proprietary 2.4GHz RF communication
* Factory-paired USB dongles

If the dongle is lost:

* Keyboard becomes unusable
* Creates electronic waste
* Users are forced to buy a new keyboard

---

# Proposed Solution

Develop a compact ESP32-based adapter capable of:

1. Detecting RF keyboard signals
2. Pairing using a special key combination
3. Translating RF packets into Bluetooth HID reports
4. Acting as a universal Bluetooth keyboard receiver

---

# Key Features

* Bluetooth keyboard output
* Portable compact design
* USB rechargeable
* Pairing using long press ESC key
* Future support for multiple keyboards
* Low-cost embedded solution

---

# Hardware Requirements

## Main Components

| Component               | Purpose                             |
| ----------------------- | ----------------------------------- |
| ESP32 Dev Board         | Main microcontroller with Bluetooth |
| NRF24L01+ Module        | RF receiver for keyboard packets    |
| Li-ion Battery          | Portable power                      |
| TP4056 Module           | Battery charging                    |
| OLED Display (Optional) | Status display                      |
| Push Button             | Manual pairing/reset                |

---

# System Architecture

Keyboard RF Signal
↓
NRF24L01 Receiver
↓
ESP32 Processing
↓
Bluetooth HID Output
↓
Laptop / Mobile / Tablet

---

# Working Principle

## Pairing Mode

1. User powers ON KeyRescue
2. User long presses ESC key on keyboard
3. ESP32 enters RF scan mode
4. RF packets are captured
5. Device stores keyboard address
6. Bluetooth pairing becomes available

---

# Supported Keyboard Types

## Initial Support

This prototype mainly targets keyboards using:

* NRF24L01
* SI24R1
* BK2421

Generic low-cost keyboards have a higher chance of compatibility.

---

# Challenges

## RF Reverse Engineering

Each manufacturer uses different:

* Packet structures
* Channels
* Addresses
* Encryption methods

Modern keyboards may use encryption making universal compatibility difficult.

---

# Future Improvements

* OLED status display
* Multi-keyboard support
* Auto RF detection
* USB HID support
* Better battery management
* Custom PCB
* AI-assisted RF analysis

---

# Software Libraries

## ESP32 BLE Keyboard Library

[https://github.com/T-vK/ESP32-BLE-Keyboard](https://github.com/T-vK/ESP32-BLE-Keyboard)

## RF24 Library

[https://github.com/nRF24/RF24](https://github.com/nRF24/RF24)

---

# Folder Structure

```bash
KeyRescue/
│
├── README.md
├── src/
│   └── main.ino
├── docs/
│   └── architecture.png
├── hardware/
│   └── circuit_diagram.png
└── libraries/
```

---

# ESP32 Example Code

File: src/main.ino

```cpp
#include <BleKeyboard.h>
#include <SPI.h>
#include <RF24.h>

// Bluetooth keyboard object
BleKeyboard bleKeyboard("KeyRescue", "SpideyTech", 100);

// NRF24 pins
RF24 radio(4, 5); // CE, CSN

// Pipe address
const byte address[6] = "00001";

char receivedChar;

void setup() {
  Serial.begin(115200);

  // Start Bluetooth keyboard
  bleKeyboard.begin();

  // Start NRF24
  radio.begin();
  radio.openReadingPipe(0, address);
  radio.setPALevel(RF24_PA_LOW);
  radio.startListening();

  Serial.println("KeyRescue Started");
}

void loop() {

  // Check RF data
  if (radio.available()) {

    radio.read(&receivedChar, sizeof(receivedChar));

    Serial.print("Received: ");
    Serial.println(receivedChar);

    // Send as Bluetooth keyboard
    if (bleKeyboard.isConnected()) {

      switch(receivedChar) {

        case 'A':
          bleKeyboard.print("A");
          break;

        case 'B':
          bleKeyboard.print("B");
          break;

        case 'C':
          bleKeyboard.print("C");
          break;

        case '1':
          bleKeyboard.print("1");
          break;

        case '2':
          bleKeyboard.print("2");
          break;

        case '3':
          bleKeyboard.print("3");
          break;

        case 'E':
          bleKeyboard.press(KEY_ESC);
          delay(100);
          bleKeyboard.releaseAll();
          break;

        default:
          break;
      }
    }
  }
}
```

---

# Linux Folder Creation Commands

```bash
cd ~
mkdir KeyRescue
cd KeyRescue
mkdir src docs hardware libraries
nano README.md
```

Save the ESP32 code as:

```bash
nano src/main.ino
```

---

# Arduino IDE Setup

## Install Required Libraries

Open Arduino IDE:

1. Sketch → Include Library → Manage Libraries
2. Install:

   * ESP32 BLE Keyboard
   * RF24

---

# Upload Instructions

1. Connect ESP32 using USB
2. Select ESP32 board
3. Select correct COM/USB port
4. Click Upload

---

# Applications

* Reviving old keyboards
* E-waste reduction
* Embedded systems learning
* RF protocol research
* Bluetooth HID projects
* Electronics hackathons

---

# Author

Developed by Spidey

Embedded Systems | RF Communication | ESP32 | Bluetooth HID
