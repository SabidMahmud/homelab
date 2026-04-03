## Storage Provisioning
### Current Solution: 
local-path-provisioner Default K3s storage class is used for persistent volumes (PV). 
* **Path:** `/var/lib/rancher/k3s/storage` 
### Planned: 
Longhorn Plan to implement Longhorn for block storage replication between the ideaserver node and the Pibox to ensure high availability for the databases.
