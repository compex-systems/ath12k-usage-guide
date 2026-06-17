# How to set up monitor mode with ath12k
This repository provides a step-by-step guide and necessary patches to enable monitor mode and packet capturing on Qualcomm-based Wi-Fi 7 modules using the `ath12k` driver.

## Support Hardware
* Compex Wi-Fi 7000 series(QCN9274)
* Tested on Ubuntu 22.04 with mainline **Kernel 6.16 or newer**

## 1. Prerequisites
Ensure you have the necessary build tools and kernel headers installed:
```bash
sudo apt update
sudo apt install tcpdump iw iproute2 -y
```

## 2. some patch about monitor mode before mainline kernel 7.1
Since native monitor mode might have issues in earlier mainline kernels, you may need to apply specific driver patches.

Please refer to the 'patches' folder

## 3. Set-up step 
Below is an example using the WLE7002E25 module.
Note: First, run iw dev to identify your correct phy index (e.g., phy0) and network interface name (e.g., wlan0).
```bash
sudo iw phy phy0 interface add mon0 type monitor
sudo ip link set wlan0 down
sudo ip link set mon0 up
#Set the sniffer to a specific frequency (5745 MHz for 5GHz CH149)
sudo iw dev mon0 set freq 5745
```

## 4. Start capture packets
```bash
sudo tcpdump -i mon0 -w ath12k.pcap
```
