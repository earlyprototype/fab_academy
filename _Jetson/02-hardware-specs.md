# Jetson Orin Nano Hardware Specifications

## Core Specifications

### Processor
- **GPU**: 1024-core NVIDIA Ampere architecture GPU
- **CPU**: 6-core Arm Cortex-A78AE v8.2 64-bit CPU
  - CPU Max Frequency: 1.5 GHz
- **Memory**: 8GB 128-bit LPDDR5
  - Bandwidth: 68 GB/s
- **Storage**: microSD card slot (no onboard eMMC)

### AI Performance
- **AI Performance**: 40 TOPS (Tera Operations Per Second)
- **GPU Architecture**: Ampere with 2nd gen Tensor Cores
- **DLA (Deep Learning Accelerator)**: 2x NVDLA v2 engines
- **Vision Accelerator**: 1x PVA (Programmable Vision Accelerator) v2

## Connectivity & I/O

### Network
- **Ethernet**: 1x Gigabit Ethernet (10/100/1000BASE-T)
- **WiFi/Bluetooth**: M.2 Key E slot for wireless module (not included)

### USB
- **USB 3.2**: 4x USB 3.2 Gen2 Type-A ports
- **USB 2.0**: 1x USB 2.0 Micro-B (device mode, also used for serial console)

### Display
- **DisplayPort**: 1x DisplayPort 1.2 output
  - Supports up to 4K @ 30Hz via HDMI adapter
  - 1080p @ 60Hz recommended for development
  - **No native HDMI port** - requires DisplayPort to HDMI adapter if using HDMI displays
  - USB-C port does NOT support video output
- **eDP**: 1x eDP 1.4a (for embedded displays)

### Camera
- **CSI**: 2x MIPI CSI-2 camera connectors (22-pin)
  - Supports up to 2x cameras
  - Max resolution: Dependent on camera module

### GPIO & Expansion
- **GPIO Header**: 40-pin expansion header (Jetson GPIO)
  - Compatible with many Raspberry Pi HATs (voltage-dependent)
  - Includes: GPIO, I2C, SPI, UART, PWM, I2S
- **M.2 Key M**: PCIe x4 slot for NVMe SSD
- **Fan Header**: 4-pin PWM fan connector

### Other Interfaces
- **I2C**: Multiple I2C buses exposed on header
- **SPI**: SPI bus on header
- **UART**: Multiple UART interfaces
- **I2S**: Audio interface
- **CAN**: CAN bus support

## Power

### Power Supply
- **Input**: DC barrel jack (5.5mm OD, 2.5mm ID)
- **Voltage**: 7V - 20V
- **Recommended**: 19V / 4.74A (90W) power adapter
- **Typical Power Draw**: 7W - 25W (depending on workload)

### Power Modes
Multiple power modes available for optimizing performance vs power:
- **MAXN**: Maximum performance (25W)
- **15W**: Balanced mode (15W)
- **10W**: Power-saving mode (10W)

## Physical Specifications
- **Dimensions**: 103mm x 90mm x 35mm (approximately)
- **Operating Temperature**: 0°C to 50°C (with proper cooling)
- **Cooling**: Active cooling (fan) recommended for sustained workloads

## Software Support
- **JetPack SDK**: Complete development environment
- **CUDA**: GPU-accelerated computing
- **cuDNN**: Deep learning primitives library
- **TensorRT**: High-performance deep learning inference
- **OpenCV**: Computer vision library with CUDA acceleration
- **ROS/ROS2**: Robot Operating System support

## Capabilities Relevant to Meshtastic Project

### Strengths for Project
1. **Processing Power**: Sufficient for coordinating multiple Meshtastic nodes
2. **Network**: Gigabit Ethernet + WiFi expansion for backbone network
3. **GPIO**: Direct hardware interfacing with Meshtastic radios
4. **Low Power Options**: Can run in 10W mode for field deployment
5. **USB Ports**: Multiple radios can be connected simultaneously
6. **AI Capabilities**: Potential for intelligent mesh routing algorithms

### Considerations
- **Power Requirements**: Higher than typical embedded boards (need appropriate power solution)
- **Heat Management**: May need active cooling depending on workload
- **Physical Size**: Larger than typical IoT devices (consider for enclosure design)
- **No Built-in Wireless**: Requires additional module for WiFi/Bluetooth

## Testing Priorities
Based on project needs, prioritise testing:
1. GPIO reliability for radio interfacing
2. Network throughput for mesh coordination
3. USB stability with multiple devices
4. Power consumption in different modes
5. Thermal performance under sustained load

## Resources
- [Official Jetson Orin Nano Documentation](https://developer.nvidia.com/embedded/jetson-orin-nano)
- [Jetson GPIO Library](https://github.com/NVIDIA/jetson-gpio)
- [JetPack SDK Documentation](https://docs.nvidia.com/jetpack/)

---
*Notes and observations to be added during testing*
