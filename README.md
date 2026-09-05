# grad-project

# Security Solution Resource Requirements

| Solution | Role | CPU | RAM | Storage | Notes |
|---|---|---|---|---|---|
| **Splunk SIEM** | Log collection, indexing, correlation, search | 8–16+ cores (scales with log volume) | 32–64 GB | Fast NVMe/SSD, RAID10 — (scales with log volume) | Heaviest component |
| **FortiGate Firewall** | Perimeter security, traffic inspection | 2–4 cores | 4–8 GB | Minimal | - |
| **Trend Micro EDR (Management Console)** | Aggregates agent alerts/status from all endpoints | 4–8 cores | 16–32 GB | Needs a SQL database backend (add ~50–100 GB for DB growth) | This is the server-side console — not the endpoint agent |
| **Windows Server + Active Directory** | Identity, authentication, group policy | 2–4 cores | 8–16 GB | 100–200 GB | Light load, but availability-critical |
| **BIG-IP WAF** | Web app firewall, load balancing | 2–4 cores | 4–8 GB | Minimal | - |
| **Windows Endpoint** (OS + EDR agent) | Regular workstation running the EDR agent | 2–4 cores | 8–16 GB total (agent itself only adds ~200–300 MB on top) | Local disk, not central server storage | - |
| **Linux Endpoint** (OS + EDR agent) | Regular workstation/server running the EDR agent | 1–4 cores | 2–8 GB total (agent itself only adds ~100–200 MB on top) | Local disk, not central server storage | - |

## Sum of Averages (per single instance of each row)

| Item | Avg CPU (cores) | Avg RAM (GB) |
|---|---|---|
| Splunk SIEM | 12 | 48 |
| FortiGate (VM) | 3 | 6 |
| Trend Micro Console | 6 | 24 |
| Windows Server + AD | 3 | 12 |
| BIG-IP (VE) | 3 | 6 |
| 2× Windows Endpoint | 6 | 24 |
| 2× Linux Endpoint | 5 | 10 |
| **Total** | **38 cores** | **130 GB** |

