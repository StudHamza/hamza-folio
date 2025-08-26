---
layout: post
title: End of My GSoC journey 
date: 2025-08-25 1:00:00
description: Final report summing up my 14 week GSOC journey with GNU Radio organization.
tags: gnuradio
categories: gsoc-updates
featured: true
images:
  compare: true
  slider: true
---

# GSoC 2025 FM Receiver App - 13-Week Progress Summary

## Project Overview
**Project**: User-friendly FM Broadcast Receiver Application with RDS and Spectrum Scanning  
**Organization**: GNU Radio  
**Timeline**: May 30 - August 25, 2025 (13 weeks)  
**Repository**: [GNU-Radio-FM-App](https://github.com/StudHamza/GNU-Radio-FM-App)  
**Documentation**: [Project Documentation](https://studhamza.github.io/GNU-Radio-FM-App/)  
**Tutorial**: [GNU Radio Tutorial](https://wiki.gnuradio.org/index.php?title=GNU_Radio_Flowgraph_Embedded_in_Python_Applications)

## Gallery
<swiper-container keyboard="true" navigation="true" pagination="true" pagination-clickable="true" pagination-dynamic-bullets="true" rewind="true">
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week14/rds_rx-1.png"  class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week9/top_menu.png" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week11/station_btn.png" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week5/rf_band.png" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week5/water_fall.png" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week5/rds.png"  class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/gnu_radio/week5/pilot_tone.png"  class="img-fluid rounded z-depth-1" %}</swiper-slide>
</swiper-container>

## Key Achievements

### Core Application Development
- **Complete PyQt5-based GUI** with home view, debug view, and SDR configuration
- **Integrated GNU Radio flowgraphs** with Python application for real-time FM reception
- **RDS (Radio Data System) integration** using gr-rds module for metadata extraction
- **Multi-threaded architecture** preventing UI freezing during scanning operations
- **Configuration management system** for saving user settings and station lists

### Advanced Features
- **Automated FM station detection** using power spectral density analysis with frequency raster alignment
- **Multiple simultaneous recording** capability with hierarchical GNU Radio blocks
- **Real-time audio visualization** and spectrum analysis in debug view
- **SDR device auto-detection** with SoapySDR integration
- **Station management** with add/delete/record functionality

### Technical Implementation
- **Modular project structure** with clear separation of GUI, core processing, and utilities
- **Custom GNU Radio embedded blocks** for station detection and signal processing
- **Frequency scanning algorithm** capable of detecting 23+ FM stations
- **Audio recording system** with WAV file output
- **Real-time spectrum waterfall** and constellation diagrams

## Weekly Progress Breakdown

### Weeks 1-2: Foundation & Design
- Established comprehensive project directory structure
- Created UI mockups in Figma and implemented basic PyQt5 interface
- Set up development environment with GNU Radio integration

### Weeks 3-4: Core FM Reception
- Implemented basic FM receiver flowgraph integration
- Added RDS panel integration from gr-rds module
- Developed initial frequency scanning approach using PSD analysis

### Weeks 5-6: Station Detection
- Built sophisticated FM scanner using frequency bin power summation
- Implemented threshold detection with peak finding algorithms
- Added debug view with RF band, waterfall, RDS, and audio visualizations
- Integrated 19kHz pilot tone detection for station verification

### Weeks 7-8: Code Quality & Features
- Code formatting and documentation using pylint, isort, and mkdocs
- Added audio visualization and improved RDS panel integration
- Resolved import issues with embedded GNU Radio blocks

### Weeks 9-10: Recording & Configuration
- Implemented audio recording functionality with WAV sink integration
- Added top menu bar and file path selection
- Created SDR device configuration manager with auto-detection
- Fixed toggle button state management during scanning

### Weeks 11-12: Multi-Recording & Polish
- Developed multiple simultaneous recording feature using hierarchical blocks
- Added station button widgets with record/delete functionality
- Implemented notification system for bandwidth constraints
- Created auto-select SDR functionality for single-device setups

### Week 13: Documentation & Finalization
- Comprehensive documentation with mkdocs GitHub pages deployment
- Migrated from deprecated Osmocom to soapy_custom_source blocks
- Final bug fixes and README improvements across all directories

### Week 14: Write Tutorial in GNU Radio Tutorials Section


## Technical Challenges Overcome

### SDR Resource Management
- **Problem**: GNU Radio flowgraphs couldn't share SDR access simultaneously
- **Solution**: Implemented selector blocks to switch between scan/play modes within single flowgraph

### Station Detection Accuracy
- **Problem**: Simple peak detection failed without external LNA amplification
- **Solution**: Developed normalized power summation approach aligned to FM frequency raster (100kHz intervals)

### Multi-threading Integration
- **Problem**: Blocking operations froze the GUI during spectrum scanning
- **Solution**: Created QThread-based scanning monitor with progress reporting

### Import System Conflicts
- **Problem**: GRC-generated code had import path issues with embedded blocks
- **Solution**: Manual module registration in sys.modules before flowgraph import

## System Architecture

### Directory Structure
```
fm-receiver-app/
├── docs/              # MkDocs documentation
├── flowgraphs/        # GNU Radio .grc files and generated Python
├── src/fm_receiver/   # Main application code
│   ├── gui/          # PyQt5 interface components
│   ├── core/         # GNU Radio integration
│   └── utils/        # Helper functions and scanning logic
├── tests/            # Unit and integration tests
└── examples/         # Sample configurations
```

### Key Components
- **Main Window**: Central GUI controller with view management
- **FM Scanner**: Embedded GNU Radio block for station detection
- **Multiple Recorder**: Hierarchical blocks for simultaneous recording
- **Config Manager**: Settings persistence and SDR device management
- **Debug View**: Real-time visualization with spectrum plots

## Performance Metrics

### Detection Capabilities
- **Station Detection**: Up to 23 FM stations detected automatically
- **Frequency Range**: 87.5-108.0 MHz FM broadcast band
- **Detection Method**: Power spectral density with 100kHz raster alignment
- **Accuracy**: All detected stations verified through manual listening

### Recording Features
- **Simultaneous Recordings**: Up to 3 stations within 2MHz RTL-SDR bandwidth
- **Audio Quality**: 48kHz sampling rate with WAV output
- **Bandwidth Constraints**: Automatic validation prevents out-of-range recording

### Real-time Processing
- **FFT Size**: 1024-point FFT for spectrum analysis
- **Update Rate**: Real-time spectrum waterfall and audio visualization
- **Threading**: Non-blocking GUI with background processing threads

## Future Enhancement Opportunities

### Short-term Improvements
1. **Dynamic Sample Rate Control**: Allow users to change SDR sample rates without flowgraph reconstruction
2. **Enhanced FM Detection**: Statistical noise testing to prevent false positives
3. **Station Strength Indicators**: Display signal strength metrics for detected stations

### Long-term Extensions
1. **SCA Decoding**: Subsidiary Communications Authorization support
2. **Network Integration**: Web SDR connectivity for remote reception
3. **Advanced Audio Processing**: Stereo/mono switching, audio effects


## Documentation & Resources

### User Documentation
- **GitHub Pages**: Comprehensive tutorials and API documentation
- **README Files**: Directory-specific documentation for contributors
- **Installation Guide**: Step-by-step setup instructions
- **Usage Examples**: Sample configurations for different SDR hardware

### Developer Resources
- **Code Documentation**: Comprehensive docstrings and type hints
- **Testing Framework**: Unit tests for core functionality
- **Contribution Guidelines**: Clear project structure for new developers

## Conclusion

The FM Receiver Application successfully demonstrates a complete software-defined radio solution combining GNU Radio's signal processing capabilities with a user-friendly PyQt5 interface. The project achieved all primary objectives including automated station detection, RDS integration, recording functionality, and advanced debugging tools.

The modular architecture and comprehensive documentation provide a solid foundation for future enhancements and community contributions. The application serves as both a practical FM receiver tool and an educational resource for SDR development with GNU Radio.

**Final Status**: Project completed successfully with all core features implemented and documented. The application is ready for end-user deployment and continued community development.

---

**Useful Links**

- [Project Repo](https://github.com/StudHamza/GNU-Radio-FM-App)  
- [All Weekly Blogs](https://studhamza.github.io/hamza-folio/blog/tag/gnuradio/)
