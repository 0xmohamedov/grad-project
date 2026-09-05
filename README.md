# grad-project

# Security Solution Resource Requirements

| Solution | Role | CPU | RAM | Storage | Notes |
|---|---|---|---|---|---|
| **Splunk SIEM** | Log collection, indexing, correlation, search | 8–16+ cores (scales with log volume) | 32–64 GB | Fast NVMe/SSD, RAID10 — sized by daily ingest × retention | Heaviest component; needs IOPS, not just capacity |
| **FortiGate Firewall** | Perimeter security, traffic inspection | 2–4 cores (if virtual appliance) | 4–8 GB | Minimal | Usually a dedicated physical appliance instead of a VM |
| **Trend Micro EDR (Management Console)** | Aggregates agent alerts/status from all endpoints | 4–8 cores | 16–32 GB | Needs a SQL database backend (add ~50–100 GB for DB growth) | This is the server-side console — not the endpoint agent |
| **Windows Server + Active Directory** | Identity, authentication, group policy | 2–4 cores | 8–16 GB | 100–200 GB | Light load, but availability-critical — don't starve it |
| **BIG-IP WAF** | Web app firewall, load balancing | 2–4 cores (if virtual edition) | 4–8 GB | Minimal | Usually a dedicated physical appliance instead of a VM |
| **Windows Endpoint** (OS + EDR agent) | Regular workstation/server running the EDR agent | 2–4 cores | 8–16 GB total (agent itself only adds ~200–300 MB on top) | Local disk, not central server storage | The OS needs the 8–16 GB, not the agent — agent overhead is negligible by comparison |
| **Linux Endpoint** (OS + EDR agent) | Regular workstation/server running the EDR agent | 1–4 cores | 2–8 GB total (agent itself only adds ~100–200 MB on top) | Local disk, not central server storage | Linux can run leaner than Windows, but 2 GB is a bare minimum, not typical |

## Sum of Averages (per single instance of each row)

| Item | Avg CPU (cores) | Avg RAM (GB) |
|---|---|---|
| Splunk SIEM | 12 | 48 |
| FortiGate (VM) | 3 | 6 |
| Trend Micro Console | 6 | 24 |
| Windows Server + AD | 3 | 12 |
| BIG-IP (VE) | 3 | 6 |
| 1× Windows Endpoint | 3 | 12 |
| 1× Linux Endpoint | 2.5 | 5 |
| **Total** | **32.5 cores** | **113 GB** |

**Important:** the endpoint rows above are per single machine. This total assumes only ONE Windows and ONE Linux endpoint — it does NOT multiply by your actual endpoint count. For your real fleet, take the endpoint RAM/CPU per machine and multiply by however many Windows/Linux machines you have; that's local to each device, not something your central server needs to provide. Only the top five rows (Splunk, FortiGate, Trend Micro console, AD, BIG-IP) are what your central server actually needs to host.
