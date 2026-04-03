## Network Topology and IP Schema

### Gateway Configuration
* **Router:** Generic Home Router
* **Gateway IP:** 192.168.0.1
* **Subnet:** 192.168.0.0/24

### Static IP Assignments
| Device Name | IP Address    | Connection | Role              |
| :---------- | :------------ | :--------- | :---------------- |
| ideaserver  | 192.168.0.200 | Ethernet   | K3s Control Plane |
| pibox       | 192.168.0.202 | Ethernet   | K3s Worker Node   |
| atlas       | 192.168.0.199 | Wi-Fi      | Management Laptop |

### Virtual IP Pool (MetalLB)
The following range is reserved for Kubernetes LoadBalancer services:
* **Range:** 192.168.0.50 - 192.168.0.70
* **Protocol:** Layer 2 (ARP)