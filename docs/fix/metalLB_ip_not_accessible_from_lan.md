# MetalLB LoadBalancer Connectivity Fix

## Issue Description
External devices were unable to reach Kubernetes `LoadBalancer` services (e.g., Traefik on `192.168.0.50`). Internal cluster logs showed significant "ARP flapping" and `memberlist` gossip errors:
`memberlist: Decrypt packet failed: Unsupported encryption version 88 from=192.168.0.200:50445`

## Root Causes
1. **L2 ARP Issues over Wi-Fi:** The `ideaserver` (control plane) is connected via Wi-Fi. MetalLB's Layer 2 mode relies on ARP responses for multiple IPs from a single MAC address, which many Wi-Fi access points block or drop (Client/AP Isolation).
2. **Gossip Encryption Mismatch:** An inconsistent or corrupted `memberlist` secret in the `metallb-system` namespace caused nodes to fail peer-to-peer coordination (port 7946), leading to split-brain scenarios where both nodes tried to announce the same IP or none did.

## Resolution Steps

### 1. Pinning L2 Advertisements to Ethernet
To ensure stable ARP responses, the `L2Advertisement` was restricted to the `worker1` (pibox) node, as it is connected via a stable Ethernet link.

**File:** `homelab/system/metallb/metallb-l2.yaml`
```yaml
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: homelab-l2
  namespace: metallb-system
spec:
  ipAddressPools:
    - homelab-pool
  nodeSelectors:
    - matchLabels:
        kubernetes.io/hostname: worker1
```

### 2. Regenerating Memberlist Gossip Secret
The gossip encryption was reset to resolve the decryption errors.

```bash
# Delete the existing corrupted secret
kubectl delete secret -n metallb-system memberlist

# Manually create a new 128-bit secret key
kubectl create secret generic -n metallb-system memberlist \
  --from-literal=secretkey="$(openssl rand -base64 128)"

# Restart the speaker pods to apply the new secret
kubectl rollout restart daemonset -n metallb-system speaker
```

## Verification
1. **Speaker Logs:** Confirmed "memberlist join successfully" in the speaker pods on both nodes.
2. **Service Announcement:** `kubectl describe svc traefik -n kube-system` confirms the service is consistently announced by `worker1`.
3. **External Reachability:** Verified `192.168.0.50` (Traefik) and `192.168.0.51` (Ganache) are reachable from the network.

## Recommendations
- If `ideaserver` is ever moved to a wired connection, the `nodeSelector` in `metallb-l2.yaml` can be removed to allow for high-availability failover.
- Keep port `7946` (TCP/UDP) open between nodes for MetalLB gossip.
