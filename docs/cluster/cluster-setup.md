## K3s Cluster Configuration

### Installation
K3s is installed with the default ServiceLB disabled to allow MetalLB to manage Layer 2 traffic.
#### Control Plane (ideaserver)
```bash
curl -sfL [https://get.k3s.io](https://get.k3s.io) | sh -s - server \
  --disable servicelb \
  --node-ip 192.168.0.200	
```
#### Worker Node (pibox)
```bash
curl -sfL [https://get.k3s.io](https://get.k3s.io) | K3S_URL=[https://192.168.0.200:6443](https://192.168.0.200:6443) \
  K3S_TOKEN=[YOUR_NODE_TOKEN] sh -
```

### Post Installation
* **Config Path:** `/etc/rancher/k3s/k3s.yaml`
* **MetalLB Version:** v0.13.12
