## Node: ideaserver (Control Plane)

### Hardware Specifications
```
mirok@ideaserver
OS: Ubuntu x86_64
Host: 81WE IdeaPad 3 15IIL05
Kernel: 6.8.0-106-generic
Uptime: 17 days, 15 hours, 47 mins
Shell: bash 5.2.21
CPU: Intel i3-1005G1 (4) @ 3.400GHz [46.0°on]
GPU: Intel Iris Plus Graphics G1
Memory: 1374MiB / 3473MiB
Disk (/): 79G / 233G (36%)
```

### Operating System
* **Distribution:** Ubuntu Server 24.04 LTS
* **Kernel:** 6.8.x
* **Status:** Lid-switch suspension disabled via `/etc/systemd/logind.conf`.

### Critical Configurations
* **ARP Tuning:** Applied to stabilize MetalLB announcements.
  ```bash
  sudo sysctl -w net.ipv4.conf.all.arp_ignore=1
  sudo sysctl -w net.ipv4.conf.all.arp_announce=2