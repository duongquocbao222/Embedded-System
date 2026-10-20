# Embedded System — ESP32 GPIO Control with ESP RainMaker

## 1. Overview

This repository contains an embedded-system project based on the **ESP32 platform and ESP RainMaker**, designed to demonstrate remote GPIO control through a mobile application.

The project follows the ESP RainMaker GPIO example. It defines three Boolean parameters associated with GPIO outputs. Users can change these parameters from a mobile application to control the corresponding GPIO states on the ESP32 board. External LEDs or other compatible digital-output circuits can be connected to the GPIO pins to demonstrate the results.

The project also provides a starting point for learning ESP-IDF development, IoT device provisioning, cloud-connected device control, and embedded firmware configuration.

## 2. Objectives

The main objectives of this project are to:

- Understand the development workflow of an ESP32-based embedded system.
- Use ESP-IDF and CMake to configure and build firmware.
- Explore the ESP RainMaker framework for connecting embedded devices to a mobile application.
- Control GPIO outputs remotely using Boolean parameters.
- Observe device events and parameter changes through the serial monitor.
- Understand flash partition tables and firmware configuration.
- Provide a foundation for further IoT and smart-device development.

## 3. Features

### GPIO control

The application example defines three Boolean parameters linked to GPIO outputs.

Each parameter represents an on/off state:

- `true`: the associated GPIO is configured to its active state.
- `false`: the associated GPIO is configured to its inactive state.

The exact GPIO numbers and device names depend on the application firmware and hardware configuration.

### Mobile application control

Users can change the GPIO-related parameters from the ESP RainMaker mobile application. The ESP32 processes the updated values and changes the corresponding output states.

External LEDs can be connected to suitable GPIO pins to visualize the changes.

### Serial monitoring

The serial monitor can display log messages when parameter values are received. An example message documented in the original project is:

```text
I (16073) app_main: Received value = true for GPIO-Device - Red
```

The actual log format and device name depend on the firmware implementation.

### Factory reset

The original example documents a factory-reset operation triggered by pressing and holding the board's BOOT button for more than three seconds.

After a factory reset, the device must be provisioned again before it can be used with ESP RainMaker.

> **Important:** Confirm that the BOOT-button behavior is implemented in the firmware for the specific board before relying on this operation.

## 4. System Architecture

The intended system consists of the following components:

```text
+------------------------+
| ESP RainMaker Mobile   |
| Application            |
+------------+-----------+
             |
             | Device control
             v
+------------------------+
| ESP RainMaker Service  |
+------------+-----------+
             |
             | Network communication
             v
+------------------------+
| ESP32 Development Board|
| ESP-IDF Firmware       |
+------------+-----------+
             |
             | GPIO outputs
             v
+------------------------+
| External LEDs /        |
| Digital Output Circuits|
+------------------------+
```

The mobile application changes a device parameter, the ESP RainMaker communication layer delivers the update, and the firmware applies the corresponding GPIO state.

The precise communication, provisioning, and GPIO-handling implementation depends on the application source code.

## 5. Hardware Requirements

The following hardware is suitable for developing and testing this type of project:

| Component | Purpose |
|---|---|
| ESP32 development board | Runs the embedded firmware |
| USB data cable | Powers the board and provides serial communication |
| Computer | Runs ESP-IDF and the build tools |
| Mobile phone | Runs the ESP RainMaker application |
| LEDs and suitable current-limiting resistors | Visually demonstrate GPIO output states |
| Breadboard and jumper wires | Optional prototyping accessories |

**Hardware notes**

- Identify the exact ESP32 variant before selecting the firmware target.
- Check the board pinout before connecting external circuits.
- Verify the GPIO assignments in the application source code.
- Use suitable current-limiting resistors when connecting LEDs.
- Do not assume every GPIO is available for general-purpose output on every ESP32 variant.

## 6. Software Requirements

The repository configuration indicates that it was generated with **ESP-IDF 5.4.3**.

Recommended development environment:

- **ESP-IDF:** 5.4.3, or a compatible version after verification.
- **Build system:** CMake.
- **Compiler and build tools:** Installed through the ESP-IDF setup.
- **ESP RainMaker:** Required for the RainMaker application and device integration.
- **Mobile application:** ESP RainMaker app compatible with the configured device workflow.
- **Serial terminal:** ESP-IDF Monitor or another compatible serial monitor.

Official documentation:

- [ESP-IDF Programming Guide](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/)
- [ESP RainMaker Documentation](https://rainmaker.espressif.com/docs/)
- [ESP RainMaker — Getting Started](https://rainmaker.espressif.com/docs/get-started.html)

## 7. Repository Structure

The tracked files currently visible in the repository are organized as follows:

```text
Embedded-System/
└── Embedded System/
    ├── CMakeLists.txt
    ├── README.md
    ├── dependencies.lock
    ├── partitions.csv
    ├── partitions_4mb_optimised.csv
    ├── sdkconfig
    ├── sdkconfig.defaults
    ├── sdkconfig.defaults.esp32
    ├── sdkconfig.defaults.esp32c2
    ├── sdkconfig.defaults.esp32c5
    ├── sdkconfig.defaults.esp32c6
    ├── sdkconfig.defaults.esp32h2
    ├── sdkconfig.defaults.esp32s2
    └── sdkconfig.old
```

### File descriptions

| File | Description |
|---|---|
| `CMakeLists.txt` | Defines the CMake project and includes the ESP-IDF build system. |
| `README.md` | Contains the original GPIO example description and basic usage information. |
| `dependencies.lock` | Records versions and dependency information for ESP-IDF components. |
| `sdkconfig` | Stores the generated project configuration. |
| `sdkconfig.defaults` | Defines default settings, including a custom flash partition table and selected RainMaker-related options. |
| `sdkconfig.defaults.esp32` and other target-specific defaults | Provide configuration defaults for supported ESP32 targets. |
| `partitions.csv` | Defines flash partitions, including application slots and storage partitions. |
| `partitions_4mb_optimised.csv` | Provides an alternative partition layout intended for a 4 MB flash configuration. |
| `sdkconfig.old` | Stores a previous configuration snapshot. |

**Repository limitation:** The current tracked file list does not include the application source directory, such as `main/`, or a component source tree. These files are necessary to inspect the actual GPIO assignments and compile the complete application. The project structure above reflects the files currently visible on GitHub, not an assumption that untracked local files do not exist.

## 8. Build and Flash

### Step 1 — Install ESP-IDF

Install ESP-IDF using the official Espressif instructions and open the corresponding ESP-IDF terminal.

Use a version compatible with the project's configuration. ESP-IDF 5.4.3 is the version identified in the checked-in `sdkconfig` header.

### Step 2 — Clone the repository

```bash
git clone https://github.com/duongquocbao222/Embedded-System.git
cd Embedded-System
```

Enter the project directory:

```bash
cd "Embedded System"
```

### Step 3 — Check the application files

Before building, verify that the project contains its application entry point and source files. A typical ESP-IDF application includes files such as:

```text
main/
├── CMakeLists.txt
└── main.c
```

The exact file names and structure can differ. Restore or add the required source files if they are missing.

### Step 4 — Configure the target

Select the target that matches the actual development board. For example, if the board is an ESP32-S2, the command is:

```bash
idf.py set-target esp32s2
```

Do not use this example target unless the physical board is actually an ESP32-S2. Other ESP32 variants require their corresponding target names.

### Step 5 — Configure and build

Once the application source code and required RainMaker dependencies are available, configure the project:

```bash
idf.py menuconfig
```

Review the project settings, board target, flash configuration, and partition-table selection.

Then build:

```bash
idf.py build
```

### Step 6 — Flash the firmware

Replace `PORT` with the serial port assigned to the development board.

```bash
idf.py -p PORT flash
```

For example, a Windows system may assign a port such as `COM9`; the actual port must be checked on the computer.

### Step 7 — Open the serial monitor

```bash
idf.py -p PORT monitor
```

To flash the firmware and start the monitor in one command:

```bash
idf.py -p PORT flash monitor
```

These commands describe the standard ESP-IDF workflow. They cannot be confirmed to complete successfully with the repository's currently visible files because the application source code is not present in the tracked tree.

## 9. Device Provisioning and Usage

After a complete firmware image has been built and flashed:

1. Power on the ESP32 development board.
2. Open the ESP RainMaker mobile application.
3. Follow the applicable provisioning procedure to connect the device to the required network and RainMaker account.
4. Wait for the device to become available in the application.
5. Open the device controls and change the configured GPIO Boolean parameters.
6. Observe the output changes on the board or connected LEDs.
7. Check the serial monitor for corresponding firmware log messages.

The exact onboarding procedure may differ depending on the ESP32 variant, firmware configuration, and provisioning method.

## 10. Flash Partition Configuration

This repository contains two custom partition-table files.

### `partitions.csv`

The table defines partitions for secure certificate data, encrypted NVS key storage, general NVS storage, OTA metadata, PHY initialization data, two OTA application slots, and a factory partition.

### `partitions_4mb_optimised.csv`

This alternative layout is configured for a 4 MB flash device and uses larger OTA application slots. It also changes the location of the factory partition.

The selected partition table must match the device's flash capacity and the firmware configuration. An incompatible layout can cause flashing errors or leave insufficient space for the application.

The presence of a partition table does not by itself prove that secure boot, flash encryption, or OTA updating has been fully implemented and tested in the application.

## 11. Troubleshooting

### Build fails because source files are missing

**Possible cause:** The repository does not contain the application source directory or its required CMake configuration.

**Suggested action:** Check the complete local project and restore the necessary source files before building.

### Incorrect target selected

**Possible cause:** The configured ESP-IDF target does not match the physical board.

**Suggested action:** Verify the board model and select the corresponding target before rebuilding.

### Device does not appear in the mobile application

**Possible causes:**
- Provisioning has not been completed.
- The device cannot connect to the configured network.
- The firmware or RainMaker configuration is incomplete.
- The board is running a different firmware image.

**Suggested action:** Check the serial monitor, provisioning status, network connection, and application configuration.

### GPIO output does not change

**Possible causes:**
- The GPIO number is incorrect.
- The pin is unavailable for the intended purpose on the selected board.
- The firmware does not handle the parameter update.
- The external LED or circuit is connected incorrectly.

**Suggested action:** Verify the source code, board pinout, circuit connections, and serial logs.

### Flashing or partition errors

**Possible cause:** The selected flash size or partition table does not match the board configuration.

**Suggested action:** Confirm the actual flash capacity and review the selected partition-table file and ESP-IDF configuration.

## 12. Future Improvements

Possible extensions for this project include:

- Add source-code documentation and a clear module structure.
- Define descriptive device names and GPIO mappings.
- Add local button control alongside mobile application control.
- Add status feedback through LEDs or a display.
- Introduce sensor monitoring and additional device parameters.
- Improve error handling and connection-status reporting.
- Document the hardware wiring and supported board models.
- Add build instructions and test results for a verified hardware configuration.

These are proposed improvements, not claims about features already implemented.

## 13. References

- [ESP-IDF Documentation](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/)
- [ESP RainMaker Documentation](https://rainmaker.espressif.com/docs/)
- [ESP RainMaker Getting Started](https://rainmaker.espressif.com/docs/get-started.html)
- [Project Repository](https://github.com/duongquocbao222/Embedded-System)

## 14. Project Status

The repository contains ESP-IDF configuration files and documentation describing a GPIO-control example integrated with ESP RainMaker.

The complete application source code, exact GPIO assignments, hardware configuration, and successful build or test results still need to be verified before the project can be documented as a fully reproducible implementation.

