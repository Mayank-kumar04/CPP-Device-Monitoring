# C++ Device Monitor

A TCP-based device monitoring system developed in **C++17** using **Boost.Asio**. The project simulates industrial device communication by receiving telemetry data, validating sensor values, detecting abnormal conditions, and logging monitoring events.

## Overview

The system follows a simple client-server architecture:

```text
Device Client
      |
      | TCP
      ↓
Monitoring Server
      |
      ↓
Data Parser
      |
      ↓
Validation & Alert Detection
      |
      ↓
Event Logger
```

## Features

* TCP client-server communication using Boost.Asio
* Structured device telemetry messages
* Telemetry parsing and validation
* Temperature and pressure monitoring
* Threshold-based alerts
* Timestamped event logging
* Multiple device/sensor simulation
* Parser unit tests
* Network verification using `tcpdump`

## Telemetry Format

The client sends data in the following format:

```text
DEVICE:sensor1;TEMP:40;PRESSURE:113;STATUS:OK
```

The server extracts:

```text
Device   → sensor1
Temperature → 40
Pressure    → 113
Status      → OK
```

## Alert Rules

| Parameter   | Condition | Alert                     |
| ----------- | --------- | ------------------------- |
| Temperature | > 35      | High temperature detected |
| Pressure    | > 110     | High pressure detected    |

For example:

```text
TEMP:40
PRESSURE:113
```

will generate both temperature and pressure alerts.

## Project Structure

```text
cpp-device-monitor/
│
├── include/              # Header files
├── src/                  # Server, client, parser and logger
├── tests/                # Parser unit tests
├── logs/                 # Generated log files
├── build/                # Compiled binaries
├── SPEC.md               # System specification
├── TEST_PROCEDURE.md     # Testing instructions
└── README.md             # Project documentation
```

## Requirements

* C++17 or later
* Boost.Asio
* Clang++ or another C++17-compatible compiler
* tcpdump (for network inspection)

## Build

Create the build directory:

```bash
mkdir -p build
```

### Compile Server

```bash
clang++ -std=c++17 src/server.cpp src/parser.cpp src/logger.cpp -o build/server
```

### Compile Client

```bash
clang++ -std=c++17 src/client.cpp -o build/client
```

### Compile Tests

```bash
clang++ -std=c++17 tests/test_parser.cpp src/parser.cpp -o build/test_parser
```

## Run

Start the server:

```bash
./build/server
```

Open another terminal and start the client:

```bash
./build/client
```

Run the parser tests:

```bash
./build/test_parser
```

## Testing

The project includes tests for:

* Valid telemetry messages
* Invalid and malformed input
* Temperature and pressure alerts
* Parser behavior
* Log generation
* TCP communication

Detailed testing steps are available in:

```text
TEST_PROCEDURE.md
```

## Network Validation

TCP communication can be inspected on Windows using **Wireshark**.

### Using Wireshark

1. Install Wireshark.
2. Start the device monitoring server.
3. Start the client in another terminal.
4. Open Wireshark.
5. Select the active network interface.
6. Start packet capture.
7. Use the following display filter:

```text
tcp.port == 8081
## Example

### Client

```text
Sent:
DEVICE:sensor1;TEMP:40;PRESSURE:113;STATUS:OK

Server reply:
ACK: data received with alerts
```

### Server

```text
Parsed Data:
DEVICE   = sensor1
TEMP     = 40
PRESSURE = 113
STATUS   = OK

WARNING: High temperature detected!
WARNING: High pressure detected!
```

## Engineering Highlights

* Modular separation of networking, parsing, validation, and logging
* Defensive handling of malformed telemetry data
* TCP-based client-server communication
* Threshold-based monitoring and alert generation
* Unit testing for the parser
* Network-level verification using packet inspection

## Future Improvements

* Asynchronous Boost.Asio server
* Support for multiple concurrent clients
* Configurable monitoring thresholds
* JSON-based configuration
* Modbus or OPC-UA integration
* Web-based monitoring dashboard
* TLS-secured communication

## Author

**Mayank Kumar**

B.Tech Computer Science and Engineering
Lovely Professional University


