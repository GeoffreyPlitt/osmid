# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

osmid is a lightweight, portable tool for converting between MIDI and OSC (Open Sound Control) protocols. It consists of two main executables:
- `m2o`: MIDI to OSC conversion
- `o2m`: OSC to MIDI conversion

The project is built with C++14 and uses CMake as the build system. It's used as the MIDI communication layer in Sonic Pi.

## Build Commands

### Initial Build Setup
```bash
mkdir build
cd build
cmake ..
make
```

### Platform-Specific Build
- **Windows**: `cmake -G "Visual Studio 16 2019" -A x64 ..`
- **Linux/Mac**: `cmake ..` then `make`

### Code Quality Checks
- **Whitespace check**: `python utils/check_whitespace.py` (checks for trailing whitespace and tab indentation)

## Architecture

### Core Components
- **src/m2o.cpp**: Main entry point for MIDI-to-OSC converter
- **src/o2m.cpp**: Main entry point for OSC-to-MIDI converter
- **src/midicommon.h/.cpp**: Base class for MIDI device management with sticky ID system
- **src/midiin.h/.cpp**: MIDI input device wrapper using JUCE
- **src/midiout.h/.cpp**: MIDI output device wrapper using JUCE
- **src/oscin.h/.cpp**: OSC input handling using oscpack
- **src/oscout.h/.cpp**: OSC output handling using oscpack
- **src/midiinprocessor.h/.cpp**: Processes incoming MIDI and converts to OSC
- **src/oscinprocessor.h/.cpp**: Processes incoming OSC and converts to MIDI
- **src/monitorlogger.h**: Singleton logger that can output to both console and OSC
- **src/utils.h/.cpp**: String utilities for OSC address formatting

### Key Design Patterns
- **Sticky ID System**: MIDI devices maintain consistent IDs across disconnections/reconnections via MidiCommon base class
- **Template-based OSC Addressing**: Customizable OSC address patterns using variables like $n (port name), $i (port id), $c (channel), $m (message type)
- **Singleton Logger**: MonitorLogger provides centralized logging to both console and OSC destinations
- **Processor Pattern**: Separate processor classes handle the conversion logic between MIDI and OSC

### Dependencies (all included in tree)
- **JUCE**: Cross-platform audio/MIDI framework (JuceLibraryCode/)
- **oscpack**: OSC message handling and UDP networking (external_libs/oscpack_1_1_0/)
- **spdlog**: Fast logging library (external_libs/spdlog-0.11.0/)
- **cxxopts**: Command-line option parsing (external_libs/cxxopts/)

### Platform Support
- Windows (MSVC 2015+)
- Linux (gcc 4.9+, requires ALSA)
- macOS (clang 5.1+)

## Development Notes

### Code Style
- Uses spaces for indentation (no tabs)
- No trailing whitespace
- C++14 standard
- Headers use `#pragma once`

### Testing
- No formal test suite for the main application
- External libraries include their own tests
- Manual testing via MIDI device interaction and OSC message monitoring

### Logging Levels
0-6 scale where smaller numbers = more verbose output

### Version Management
Version numbers defined in src/version.h (currently 0.8.0)