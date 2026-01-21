# Getting Started with Jetson Orin Nano Developer Kit

## IMPORTANT: Fresh-from-Box Firmware Update Required

**If your Jetson Orin Nano is fresh from the box**, you must update the firmware before installing JetPack 6.2.1. The factory firmware only supports JetPack 5.x.

Follow the **Initial Setup Guide for Jetson Orin Nano Developer Kit** from NVIDIA before proceeding:
- Guide available at: https://developer.nvidia.com/embedded/learn/get-started-jetson-orin-nano-devkit
- This is a one-time firmware update to enable JetPack 6.x compatibility

After firmware update is complete, return to this guide and continue below.

---

## Initial Setup Checklist

### Hardware Requirements
- [ ] Jetson Orin Nano Developer Kit (main board)
- [ ] Power supply (included with dev kit - 19V/4.74A)
- [ ] Cooling fan (included with dev kit)
- [ ] MicroSD card (64GB+ recommended, UHS-1 rated) - **NOT INCLUDED**
- [ ] Display with DisplayPort OR HDMI + DP-to-HDMI adapter
- [ ] **DisplayPort to HDMI adapter** (if your display only has HDMI) - **NOT INCLUDED**
- [ ] USB keyboard and mouse
- [ ] Ethernet cable (for initial setup)
- [ ] Host computer for flashing SD card

**IMPORTANT:** The Jetson Orin Nano has **DisplayPort output only** (no HDMI port). If your monitor/TV only has HDMI input, you must purchase a DisplayPort to HDMI adapter (£8-10).

### Software Requirements
- [ ] Jetson Orin Nano SD Card Image (JetPack 6.2.1)
- [ ] Balena Etcher or similar imaging tool
- [ ] Rufus (alternative for Windows users)

## Step-by-Step Setup Process

### Step 0. Firmware Update (Fresh Units Only)
**CRITICAL:** If your unit is fresh from the box or running JetPack 5.x:
- [ ] Follow NVIDIA's Initial Setup Guide for firmware update
   - [Initial Setup Guide](https://www.jetson-ai-lab.com/tutorials/initial-setup-jetson-orin-nano/)
- [ ] Verify firmware update completed successfully
- [ ] Once updated, proceed to Step 1 below

### 1. Prepare the SD Card
1. Download JetPack 6.2.1 SD Card Image from NVIDIA:
   - Main page: https://developer.nvidia.com/embedded/jetpack-sdk-621
   - Direct link to downloads section: Look for "SD Card Image Method for Jetson Orin Nano Developer Kit"
   - Version: JetPack 6.2.1 (Jetson Linux 36.4.4)
   - File size: Approximately 10-15GB compressed
2. Flash image to microSD card using Balena Etcher (or Rufus on Windows):
   - Insert microSD card into host PC
   - Launch Balena Etcher
   - Select the downloaded image file
   - Select your microSD card as target
   - Click "Flash"
   - Wait for verification to complete
3. Safely eject SD card from host PC

### 2. Hardware Assembly
1. Insert microSD card into slot on Jetson module
2. Connect HDMI display
3. Connect USB keyboard and mouse
4. Connect Ethernet cable
5. Connect power supply (board will auto-boot)

### 3. Initial Boot and Configuration
1. Follow on-screen setup wizard
2. Set username and password
3. Configure timezone and language (UK English)
4. Connect to network
5. Complete initial system updates:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

### 4. Verify Installation
Run the following commands to verify setup:
```bash
# Check JetPack version
sudo apt-cache show nvidia-jetpack

# Check CUDA installation (CUDA 12.6)
nvcc --version

# Check TensorRT version (should be 10.3)
dpkg -l | grep tensorrt

# Check system resources
tegrastats

# Check GPIO access
sudo cat /sys/kernel/debug/tegra_gpio
```

**Expected JetPack 6.2.1 Components:**
- Jetson Linux 36.4.4 (based on Ubuntu 22.04)
- Linux Kernel 5.15
- CUDA 12.6
- TensorRT 10.3
- cuDNN 9.3
- VPI 3.2 (Vision Programming Interface)

### 5. Install Essential Development Tools
```bash
# Python development
sudo apt install python3-pip python3-dev -y

# Build essentials
sudo apt install build-essential cmake git -y

# Network tools
sudo apt install net-tools wireless-tools -y

# System monitoring
sudo apt install htop iotop -y
```

## Post-Setup Tasks
- [ ] Configure SSH for remote access
- [ ] Set up static IP address (if required)
- [ ] Install project-specific dependencies
- [ ] Clone project repositories
- [ ] Configure GPIO pins for project use
- [ ] Test camera interface (if using camera)

## Troubleshooting

### Board Won't Boot with JetPack 6.2.1
**Most Common Issue:** Firmware not updated on fresh-from-box unit
- Factory firmware only supports JetPack 5.x
- You MUST complete the firmware update first (see warning at top)
- Follow NVIDIA's Initial Setup Guide before installing JetPack 6.2.1

**Other Causes:**
- Verify power supply is correct voltage/amperage (19V/4.74A)
- Check SD card is properly seated
- Try re-flashing SD card image
- Check for LED indicators on board
- Verify SD card is working (test in another device)

### No Display Output
- Verify HDMI cable connection
- Try different display/cable
- Check display input source selection
- Some displays may take a few seconds to detect signal

### Network Issues
- Use Ethernet for initial setup (more reliable)
- Check router DHCP settings
- Verify firewall settings
- Jetson Orin Nano does NOT have built-in WiFi (requires M.2 module)

## Next Steps
Once basic setup is complete, proceed to:
1. [Hardware Specs Review](./02-hardware-specs.md)
2. [Testing Checklist](./03-testing-checklist.md)
3. [Project Integration Planning](./04-project-integration.md)

---
*Setup Date: ___________*
*JetPack Version: ___________*
*Notes:*
