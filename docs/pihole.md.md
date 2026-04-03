# Service: Pi-hole DNS

## Deployment Strategy
* **Method:** Kubernetes Deployment with `hostNetwork: true`.
* **Reasoning:** Transitioned from LoadBalancer (MetalLB) due to persistent UDP 7946 gossip timeouts and ARP flapping. Bypassing the virtual network layer resolved split-brain issues.

## Technical Resolution
1. **Host Configuration:** Disabled `systemd-resolved` on the host to free Port 53.
2. **Upstream Resolution:** Manually configured `/etc/resolv.conf` to use `8.8.8.8` to allow Gravity updates when the pod is in a bootstrap state.
3. **Internal Policy:** Set `dnsPolicy: ClusterFirstWithHostNet`.

## Access
* **DNS IP:** 192.168.0.202 (worker1)
* **Web UI:** http://192.168.0.202/admin