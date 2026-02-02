# Blaze-NT Avionics System

UCSC Rocketry's high-power rocket flight computer and data acquisition system.

## Overview

The Blaze-NT is a complete avionics system designed for high-power rocket
applications. It provides flight data logging, sensor monitoring, and telemetry
capabilities for UCSC Rocketry team's launch vehicles.

**Current Status**: Prototype hardware has been flight-tested. This repository
contains the schematic revision for the next hardware iteration.

## Features

- **Flight Data Logging**: High-rate sensor data acquisition and storage
- **Telemetry**: 433MHz radio communication for real-time data downlink
- **Sensor Suite**:
  - 9-axis IMU (accelerometer, gyroscope, magnetometer)
  - Barometric pressure sensor for altitude
  - Discrete accelerometer for high-g measurements
- **Power Management**: Battery monitoring and regulated power distribution
- **Microcontroller**: STM32F411 ARM Cortex-M4 processor
- **Firmware**: Arduino framework compatibility

## Project Structure

This KiCad project uses a hierarchical schematic design with separate sheets for
each subsystem:

```
Blaze-NT/
├── Blaze-NT.kicad_sch          # Top-level schematic (team lead managed)
├── microcontroller.kicad_sch   # STM32F411 and peripherals (team lead managed)
├── power-managment.kicad_sch   # Power regulation and distribution (power team)
├── sensors.kicad_sch           # Sensor interfaces and signal conditioning (sensors team)
├── Radio.kicad_sch             # 433MHz telemetry radio (radio team)
├── Blaze-NT.kicad_pro          # Project file
├── avionics-library/           # Custom component library (git submodule)
└── README.md
```

## Hardware Components

### Main Processor

- **STM32F411**: ARM Cortex-M4 microcontroller running the Arduino framework

### Radio System

- **433MHz Transceiver**: RFM69HCW for telemetry communication
- **Antenna**: External 433MHz antenna connector

### Sensors

- **IMU**: LSM9DS1 9-axis inertial measurement unit
- **Barometer**: MS5611 high-precision pressure sensor
- **High-G Accelerometer**: KX134 for high-acceleration events

### Power Management

- **Voltage Regulators**: Integreated bucks for efficient power conversion
- **Battery Monitoring**: Voltage and current sensing capabilities

## Development Workflow

### Team-Based Development

This project is designed for collaborative development with the following roles:

**Team Lead Responsibilities:**

- Manages top-level schematic (`Blaze-NT.kicad_sch`)
- Manages microcontroller sheet (`microcontroller.kicad_sch`)
- Coordinates integration between subsystems
- Handles final schematic review and approval

**Subteam Responsibilities:**

- **Power Team**: Works on `power-managment.kicad_sch`
- **Sensors Team**: Works on `sensors.kicad_sch`
- **Radio Team**: Works on `Radio.kicad_sch`
- Each team only modifies their assigned schematic sheet

### Version Control Notes

**Important**: KiCad schematic files are binary and do not merge well. When
conflicts occur:

1. **Latest Version Wins**: The newest schematic file will override older
   versions in merges
2. **Communication is Key**: Teams must coordinate changes to avoid overwriting
   work
3. **Branch Strategy**: Use feature branches for significant changes
4. **Regular Integration**: Team lead regularly integrates subteam changes

### Git Workflow

```bash
# Clone with submodules
git clone --recurse-submodules <repository-url>

# Or initialize submodules after cloning
git submodule update --init --recursive

# If submodule isn't working, clone directly into avionics-library folder:
cd avionics-library
git clone https://github.com/UCSCRocketry/avionics-library.git .
cd ..

# Create feature branch for subsystem changes
git checkout -b feature/power-improvements

# Make changes to your assigned sheet only
# Commit and push changes
git add power-managment.kicad_sch
git commit -m "Improve power regulation efficiency"
git push origin feature/power-improvements
```

## Build Instructions

### Prerequisites

- KiCad 9.0 or later
- Git for version control

### Opening the Project

1. Clone the repository with submodules:
   ```bash
   git clone --recurse-submodule https://github.com/UCSCRocketry/blaze-nt-hardware.git
   ```

2. Open KiCad
3. Open the project file: `Blaze-NT.kicad_pro`

### Working with Schematics

1. **Team Lead**: Open and edit `Blaze-NT.kicad_sch` and
   `microcontroller.kicad_sch`
2. **Subteams**: Open only your assigned schematic sheet
3. **Symbol Library**: Custom symbols are in
   `avionics-library/Avionics_Symbols.kicad_sym`
4. **Footprints**: Custom footprints are in
   `avionics-library/Avionics_Feet.pretty/`

## Component Library

This project uses a custom component library stored as a git submodule:

- **Repository**: `UCSCRocketry/avionics-library`
- **Symbols**: `avionics-library/Avionics_Symbols.kicad_sym`
- **Footprints**: `avionics-library/Avionics_Feet.pretty/`

To update the library:

```bash
cd avionics-library
git pull origin main
cd ..
git add avionics-library
git commit -m "Update avionics library"
```

## Contribution Guidelines

### For Subteams

1. **Stay in Your Lane**: Only modify your assigned schematic sheet
2. **Test Changes**: Verify electrical rules check (ERC) passes before
   committing
3. **Document Changes**: Update schematic notes and component values
4. **Coordinate**: Communicate with team lead before major changes

### For Team Lead

1. **Review Changes**: Check all subteam contributions before integration
2. **Maintain Top Sheet**: Update hierarchical sheet references as needed
3. **Version Control**: Manage merge conflicts and coordinate integration
4. **Quality Control**: Ensure overall schematic integrity and consistency

### Commit Message Format

Use clear, descriptive commit messages:

```
subsystem: brief description

Detailed explanation of changes if needed.
```

Examples:

- `power: add battery voltage monitoring`
- `sensors: update IMU filter configuration`
- `radio: improve antenna matching network`

## File Formats

- **Schematics**: `.kicad_sch` (binary format)
- **Project Settings**: `.kicad_pro` (JSON format)
- **Project Local**: `.kicad_prl` (local settings, gitignored)
- **Symbol Library**: `.kicad_sym` (binary format)
- **Footprints**: `.kicad_mod` (text format)

## Troubleshooting

### Common Issues

1. **Missing Symbols**: Ensure `avionics-library` submodule is initialized
2. **Schematic Won't Open**: Check KiCad version compatibility
3. **ERC Errors**: Review net connections and component properties

### Getting Help

- Contact the project team lead for integration issues
- Consult KiCad documentation for tool-specific questions
- Review existing schematic sheets for design patterns

## License

This project is proprietary to UCSC Rocketry team. Do not distribute without
permission.

---

**Note**: This hardware has been flight-tested on prototype vehicles. Always
perform thorough ground testing before flight use.

