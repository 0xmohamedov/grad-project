# grad-project

# Security Solution Resource Requirements

| Solution | Role | CPU | RAM | Storage | Notes |
|---|---|---|---|---|---|
| **Splunk SIEM** | Log collection, indexing, correlation, search | 8–16+ cores (scales with log volume) | 32–64 GB | Fast NVMe/SSD, RAID10 — sized by daily ingest × retention | By far the heaviest component; needs IOPS, not just capacity |
| **FortiGate Firewall** | Perimeter security, traffic inspection | N/A (dedicated appliance or VM appliance, not shared with other services) | 4–8 GB (VM) | Minimal | Usually a physical/virtual appliance, isolated from your other VMs |
| **Trend Micro EDR (Management Console)** | Aggregates agent alerts/status from all endpoints | 4–8 cores | 16–32 GB | Needs a SQL database backend (add ~50–100GB for DB growth) | Endpoint agents themselves are lightweight (a few % CPU/RAM per host) |
| **Windows Server + Active Directory** | Identity, authentication, group policy | 2–4 cores | 8–16 GB | 100–200 GB | Light load, but availability-critical — don't starve it |
| **BIG-IP WAF** | Web app firewall, load balancing | N/A (appliance or VE) | 4–8 GB (VE) | Minimal | Same as FortiGate — typically its own appliance/VE, not co-hosted |
| **Endpoint agents (per host)** | EDR + log forwarding on each Windows/Linux machine | Negligible (~1–5% of host CPU) | ~100–300 MB per agent | Negligible | Cost is aggregate — many endpoints forwarding logs is what drives Splunk's sizing, not the agents themselves |

**Takeaway:** Splunk sets the floor for server specs — AD and the Trend Micro console fit comfortably in the leftover headroom. FortiGate and BIG-IP are best kept off that box entirely, since they're normally dedicated appliances.
