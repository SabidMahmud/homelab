## Node: pibox (Worker Node) 
### Hardware Specifications
* **Model:** Raspberry Pi 5 (4GB RAM) 
* **Operating System:** Debian Trixie (Testing) 
### Role in Cluster 
* **Node Name:** `worker1` 
* **Primary Tasks:** Running network-critical services (Pi-hole). 
### Host Networking 
* **Note:** Port 53 is managed directly by the Pi-hole pod via `hostNetwork: true`. 
* **Action:** `systemd-resolved` disabled to prevent Port 53 conflicts.