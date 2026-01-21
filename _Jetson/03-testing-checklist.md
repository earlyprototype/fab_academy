# Jetson Orin Nano Testing Checklist

## Purpose
Systematic verification of all Jetson capabilities before integration into the final project. Document all findings for Fab Academy submission.

---

## Phase 1: Basic Hardware Verification

### Power and Boot
- [ ] **Power-on test**
  - Date tested: ___________
  - Power supply used: ___________
  - Boot time: ___________ seconds
  - Status: PASS / FAIL
  - Notes:

- [ ] **Power consumption measurement**
  - Idle: ___________ W
  - Light load: ___________ W
  - Heavy load: ___________ W
  - Status: PASS / FAIL
  - Notes:

- [ ] **Power mode switching**
  - [ ] Test MAXN mode (25W)
  - [ ] Test 15W mode
  - [ ] Test 10W mode
  - Command used: `sudo nvpmodel -m [mode]`
  - Notes:

### Display Output
- [ ] **HDMI output test**
  - Resolution tested: ___________
  - Refresh rate: ___________
  - Status: PASS / FAIL
  - Notes:

- [ ] **DisplayPort test** (if available)
  - Status: PASS / FAIL
  - Notes:

### Storage
- [ ] **SD card read/write test**
  - [ ] Read speed test: `sudo hdparm -Tt /dev/mmcblk0`
  - [ ] Write speed test: `dd if=/dev/zero of=~/testfile bs=1M count=1024`
  - Read: ___________ MB/s
  - Write: ___________ MB/s
  - Status: PASS / FAIL

- [ ] **M.2 NVMe test** (if installed)
  - Read: ___________ MB/s
  - Write: ___________ MB/s
  - Status: PASS / FAIL / N/A

---

## Phase 2: Connectivity Testing

### Network
- [ ] **Ethernet connection test**
  - [ ] Physical connection established
  - [ ] DHCP address obtained: ___________
  - [ ] Static IP configured (if needed): ___________
  - [ ] Ping test to gateway
  - [ ] Internet connectivity verified
  - Speed test: ___________ Mbps
  - Status: PASS / FAIL

- [ ] **WiFi module test** (if installed)
  - Module model: ___________
  - [ ] Driver installation verified
  - [ ] AP detection: `nmcli dev wifi list`
  - [ ] Connection established
  - IP address: ___________
  - Signal strength: ___________
  - Status: PASS / FAIL / N/A

- [ ] **SSH access setup**
  - [ ] SSH server enabled: `sudo systemctl enable ssh`
  - [ ] Remote connection tested
  - [ ] Key-based authentication configured
  - Status: PASS / FAIL

### USB
- [ ] **USB 3.2 ports test (all 4 ports)**
  - Port 1: PASS / FAIL - Device tested: ___________
  - Port 2: PASS / FAIL - Device tested: ___________
  - Port 3: PASS / FAIL - Device tested: ___________
  - Port 4: PASS / FAIL - Device tested: ___________
  
- [ ] **USB 2.0 Micro-B test**
  - [ ] Serial console access: `sudo screen /dev/ttyACM0 115200`
  - Status: PASS / FAIL

- [ ] **USB mass storage test**
  - Read speed: ___________ MB/s
  - Write speed: ___________ MB/s
  - Status: PASS / FAIL

---

## Phase 3: GPIO and Hardware Interfaces

### GPIO Testing
- [ ] **GPIO library installation**
  - [ ] Jetson.GPIO installed: `sudo pip3 install Jetson.GPIO`
  - [ ] User permissions set: `sudo usermod -aG gpio $USER`
  - Status: PASS / FAIL

- [ ] **Basic GPIO output test**
  - Pins tested: ___________
  - Test script: `python3 gpio_test.py`
  - Status: PASS / FAIL
  - Notes:

- [ ] **GPIO input test**
  - Pins tested: ___________
  - Pull-up/pull-down verified
  - Status: PASS / FAIL

- [ ] **PWM output test**
  - Pins tested: ___________
  - Frequency range: ___________
  - Status: PASS / FAIL

### I2C
- [ ] **I2C bus detection**
  - [ ] i2c-tools installed: `sudo apt install i2c-tools`
  - [ ] Buses detected: `sudo i2cdetect -l`
  - Number of buses: ___________
  - Status: PASS / FAIL

- [ ] **I2C device test** (if device available)
  - Device connected: ___________
  - Address detected: ___________
  - Read/write successful
  - Status: PASS / FAIL / N/A

### SPI
- [ ] **SPI interface test**
  - [ ] SPI enabled in device tree
  - [ ] SPI device appears: `ls /dev/spi*`
  - Status: PASS / FAIL / N/A

### UART
- [ ] **UART interface test**
  - Ports tested: ___________
  - Baud rates tested: ___________
  - Status: PASS / FAIL

---

## Phase 4: Performance and AI Capabilities

### CPU Performance
- [ ] **CPU stress test**
  - [ ] Install stress: `sudo apt install stress`
  - [ ] Run test: `stress --cpu 6 --timeout 60s`
  - [ ] Monitor with htop
  - Max temperature: ___________ °C
  - Thermal throttling observed: YES / NO
  - Status: PASS / FAIL

### GPU Performance
- [ ] **GPU information**
  - Command: `sudo tegrastats`
  - GPU frequency: ___________ MHz
  - Status: PASS / FAIL

- [ ] **CUDA verification**
  - [ ] CUDA samples compiled
  - [ ] Run deviceQuery
  - CUDA version: ___________
  - Status: PASS / FAIL

### AI/ML Testing
- [ ] **TensorRT verification**
  - Version: ___________
  - [ ] Sample inference test run
  - Status: PASS / FAIL

- [ ] **PyTorch installation test**
  - [ ] PyTorch for Jetson installed
  - Version: ___________
  - [ ] Simple model inference test
  - Inference time: ___________ ms
  - Status: PASS / FAIL / N/A

---

## Phase 5: Project-Specific Testing

### Meshtastic Integration Preparation
- [ ] **USB serial device recognition**
  - [ ] Connect Meshtastic device via USB
  - [ ] Device detected: `lsusb`
  - [ ] Serial port created: `ls /dev/ttyUSB*` or `ls /dev/ttyACM*`
  - Device path: ___________
  - Status: PASS / FAIL

- [ ] **Multiple USB devices test**
  - [ ] Connect multiple Meshtastic radios
  - Number of devices: ___________
  - All detected: YES / NO
  - Status: PASS / FAIL

- [ ] **Meshtastic Python library**
  - [ ] Install: `pip3 install meshtastic`
  - [ ] Test connection: `meshtastic --info`
  - Status: PASS / FAIL

- [ ] **GPIO to radio interface test** (for direct connection)
  - Connection type: ___________
  - Communication verified: YES / NO
  - Status: PASS / FAIL / N/A

### Network Backbone Testing
- [ ] **Simultaneous network connections**
  - [ ] Ethernet + WiFi active simultaneously
  - [ ] Routing configured
  - [ ] Packet forwarding tested
  - Status: PASS / FAIL

- [ ] **Network throughput under load**
  - [ ] iperf3 test conducted
  - Throughput: ___________ Mbps
  - Latency: ___________ ms
  - Status: PASS / FAIL

---

## Phase 6: Thermal and Reliability Testing

### Thermal Management
- [ ] **Fan installation and control**
  - Fan installed: YES / NO
  - [ ] Automatic fan control verified
  - [ ] Manual control tested: `sudo jetson_clocks --fan`
  - Status: PASS / FAIL

- [ ] **Extended thermal test**
  - [ ] Run sustained workload for 1 hour
  - Peak temperature: ___________ °C
  - Steady-state temperature: ___________ °C
  - Throttling occurred: YES / NO
  - Status: PASS / FAIL

### Stability Testing
- [ ] **24-hour stability test**
  - Start time: ___________
  - End time: ___________
  - Uptime achieved: ___________
  - Crashes/errors: ___________
  - Status: PASS / FAIL

---

## Documentation Requirements for Fab Academy

### Photos and Videos
- [ ] Board unboxing and components
- [ ] Hardware assembly process
- [ ] Powered-on system with display
- [ ] GPIO testing setup
- [ ] Final integrated setup
- [ ] Screen recordings of key tests

### Data to Collect
- [ ] Power consumption graphs
- [ ] Performance benchmarks
- [ ] Thermal profiles
- [ ] Network performance data
- [ ] Test scripts and results

### Write-Up Sections
- [ ] Hardware overview and justification
- [ ] Setup process documentation
- [ ] Test methodology description
- [ ] Results and analysis
- [ ] Integration with final project plan
- [ ] Challenges and solutions
- [ ] Future development plans

---

## Summary

**Total Tests Completed**: _____ / _____  
**Pass Rate**: _____%  
**Critical Failures**: ___________  
**Overall Status**: READY / NEEDS WORK / BLOCKED  

**Next Steps**:
1. 
2. 
3. 

**Blocker Items**:
- 

---
*Testing completed by: ___________*  
*Date: ___________*  
*Review date: ___________*
