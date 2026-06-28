# HomeLab Security Monitoring and Attack Detection Lab

## Overview

This repository documents my personal cybersecurity HomeLab built for learning, monitoring, attack detection, and bachelor thesis research.

The lab is designed to simulate realistic small-network security scenarios in a controlled and isolated environment. It is used for collecting telemetry, monitoring infrastructure, analyzing logs and network traffic, and preparing a dataset for attack detection research.

The main focus areas are:

- Linux server administration
- Proxmox VE virtualization
- Network monitoring
- IDS and network security monitoring
- Security telemetry collection
- PCAP and log analysis
- MITRE ATT&CK mapping
- Incident detection and reporting
- Dataset preparation for bachelor thesis research

This project is part of my practical preparation for junior roles in SOC, cybersecurity monitoring, Linux infrastructure, and security engineering.

---

## Project Goals

The main goals of the lab are:

1. Build a realistic virtualized home infrastructure.
2. Monitor servers, network devices, and security events.
3. Deploy tools such as Prometheus, Grafana, Suricata, Zeek, and log collectors.
4. Simulate selected security scenarios in an isolated environment.
5. Collect PCAP files, IDS alerts, Zeek logs, system logs, and monitoring metrics.
6. Map selected scenarios to MITRE ATT&CK techniques.
7. Use collected data as the basis for a bachelor thesis dataset.
8. Document the architecture, detection logic, investigation workflow, and lessons learned.

---

## Hardware

Current lab hardware:

| Component | Description |
|---|---|
| Server | Dell Precision 3630 |
| CPU | Intel Xeon E-2174G |
| RAM | 16 GB DDR4, planned upgrade to 64 GB |
| Storage | 256 GB NVMe SSD + 1 TB SATA HDD |
| Router | MikroTik hAP |
| Network | Home network with separated lab services |
| Virtualization | Proxmox VE |

Planned improvements:

- RAM upgrade to 64 GB
- Additional SSD storage for virtual machines and logs
- Dedicated storage for PCAP and dataset collection
- More isolated network segments for attack simulation

---

## High-Level Architecture

```text
                    Internet
                       |
                 ISP / Modem
                       |
                  MikroTik hAP
                       |
        --------------------------------
        |                              |
   Home Network                    Lab Network
        |                              |
   Personal devices              Proxmox VE Host
                                      |
          ------------------------------------------------
          |              |              |                 |
       monitor01       log01        attacker01        victim01
     Prometheus       Syslog /      Kali/Linux       Linux/Windows
      Grafana         Logs          test host        test target
```

The lab is built around Proxmox VE. Individual services run inside virtual machines or containers.

Main VM roles:

| Hostname | Role |
|---|---|
| `pve` | Proxmox VE hypervisor |
| `monitor01` | Monitoring server, Prometheus and Grafana |
| `log01` | Central log collection |
| `ids01` | Suricata and Zeek sensor |
| `attacker01` | Controlled attack simulation host |
| `victim01` | Test target system |
| `docker01` | Containerized services and experiments |

Exact hostnames, IP addresses, and VM roles may change as the lab evolves.

---

## Current Components

### Proxmox VE

Proxmox VE is used as the main virtualization platform.

Used for:

- Managing Linux virtual machines
- Testing isolated environments
- Creating snapshots before experiments
- Separating monitoring, logging, IDS, attacker, and victim systems
- Running services required for bachelor thesis research

Planned improvements:

- Better VM templates
- Automated VM provisioning
- Separate virtual networks for attacker, victim, monitoring, and management traffic

---

### Linux Servers

Linux is the main operating system used in the lab.

Current usage:

- Server administration
- SSH access
- Bash scripting
- Service configuration
- Log collection
- Network testing
- Monitoring agents

Technologies used:

- Debian or Ubuntu-based systems
- SSH
- systemd
- Bash
- curl
- Git
- tcpdump
- basic firewall configuration

---

### Monitoring: Prometheus and Grafana

Prometheus is used for collecting metrics from lab systems.

Grafana is used for visualization and dashboards.

Currently monitored or planned metrics:

- Linux host metrics
- CPU usage
- Memory usage
- Disk usage
- Network traffic
- Service availability
- Exporter status

Exporters:

- `node_exporter` for Linux hosts
- SNMP exporter or SNMP monitoring for network devices, where applicable

Example monitoring goals:

- Detect unavailable hosts
- Track resource usage during security simulations
- Observe network or system changes during testing
- Build dashboards for infrastructure visibility

---

### MikroTik Monitoring

The MikroTik router is used as the central network device in the lab.

Monitoring goals:

- Track interface status
- Monitor network traffic
- Collect SNMP metrics
- Forward selected logs to the central log server
- Understand upstream and downstream network behavior

Planned documentation:

- MikroTik SNMP configuration
- Prometheus scrape configuration
- Grafana dashboard screenshots
- Syslog forwarding setup
- Interface traffic monitoring

---

### Log Collection

Central log collection is planned or partially implemented through a dedicated log server.

Log sources:

- Linux system logs
- SSH authentication logs
- MikroTik logs
- IDS alerts
- Application logs
- Security tool output

Example log types:

- `/var/log/auth.log`
- `/var/log/syslog`
- Suricata `eve.json`
- Zeek logs
- MikroTik syslog
- Service logs from Docker containers

Goals:

- Centralize logs from multiple systems
- Support basic investigation workflows
- Correlate system logs with network events
- Prepare log data for bachelor thesis analysis

---

### IDS and Network Security Monitoring

The lab uses or plans to use IDS and network security monitoring tools for network-based detection.

Tools:

- Suricata
- Zeek
- Wireshark
- tcpdump

Suricata is used for:

- IDS alerting
- Network event detection
- Generating structured JSON logs
- Producing alerts from simulated scenarios

Zeek is used for:

- Network protocol metadata
- Connection logs
- DNS logs
- HTTP logs
- SSL/TLS metadata
- Higher-level network visibility

Wireshark and tcpdump are used for:

- Manual PCAP analysis
- Traffic inspection
- Debugging detection logic
- Understanding network behavior

---

## Security Scenarios

The lab is used for controlled simulations of security events.

All scenarios are performed only in an isolated lab environment and only against systems owned and controlled by me.

Planned and tested scenario categories:

| Scenario | Purpose |
|---|---|
| Network scanning | Detect reconnaissance behavior |
| Service enumeration | Observe suspicious connection patterns |
| SSH authentication attempts | Analyze login activity and failed authentication |
| Vulnerability identification | Understand exposure and detection signals |
| Simulated C2-like beaconing | Detect periodic outbound communication patterns |
| Simulated data exfiltration | Analyze unusual outbound transfer behavior |
| Lateral movement simulation | Study internal movement indicators |
| Malware-like traffic replay | Analyze known PCAP samples safely |

MITRE ATT&CK mapping examples:

| Scenario | Possible MITRE ATT&CK Area |
|---|---|
| Network scanning | Reconnaissance / Discovery |
| SSH brute-force simulation | Credential Access |
| Lateral movement simulation | Lateral Movement |
| C2-like beaconing | Command and Control |
| Data exfiltration simulation | Exfiltration |

The exact ATT&CK technique IDs will be documented per scenario.

---

## Bachelor Thesis Connection

This HomeLab is connected to my bachelor thesis focused on detecting cyber attacks in a controlled security laboratory.

Working bachelor thesis topic:

> Detection of cyber attacks in a security laboratory environment

The thesis focuses on:

- Designing an isolated lab environment
- Simulating selected attack scenarios
- Collecting network traffic and system logs
- Generating IDS and SIEM alerts
- Mapping scenarios to MITRE ATT&CK
- Preparing a dataset for analysis
- Comparing the dataset with public cybersecurity datasets
- Evaluating detection quality and limitations

Dataset sources:

- PCAP captures
- Suricata alerts
- Zeek logs
- Linux logs
- Authentication logs
- Network telemetry
- Monitoring metrics

Dataset requirements:

- Reproducibility
- Clear scenario labeling
- Separation of benign and attack traffic
- Anonymization of sensitive information
- Documentation of environment and methodology

---

## Planned Dataset Structure

```text
dataset/
├── benign/
│   ├── pcap/
│   ├── logs/
│   └── metadata/
├── attacks/
│   ├── reconnaissance/
│   ├── credential_access/
│   ├── command_and_control/
│   ├── lateral_movement/
│   └── exfiltration/
├── labels/
│   ├── scenario_labels.csv
│   └── mitre_mapping.csv
└── README.md
```

Example metadata fields:

| Field | Description |
|---|---|
| Scenario ID | Unique scenario identifier |
| Date | Date of execution |
| Source host | Attacker or test host |
| Target host | Victim system |
| Tools used | Tools involved in the scenario |
| Data sources | PCAP, Zeek, Suricata, system logs |
| MITRE tactic | Mapped tactic |
| MITRE technique | Mapped technique |
| Expected detection | What should be detected |
| Actual detection | What was detected |

---

## Repository Structure

Planned repository structure:

```text
homelab-security-monitoring/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── network-diagram.md
│   ├── proxmox-setup.md
│   ├── mikrotik-monitoring.md
│   ├── prometheus-grafana.md
│   ├── log-collection.md
│   ├── suricata.md
│   ├── zeek.md
│   └── bachelor-thesis-notes.md
├── diagrams/
│   ├── network-topology.png
│   └── telemetry-flow.png
├── configs/
│   ├── prometheus/
│   ├── grafana/
│   ├── suricata/
│   ├── zeek/
│   └── mikrotik/
├── scenarios/
│   ├── 01-network-scanning.md
│   ├── 02-ssh-authentication.md
│   ├── 03-c2-beaconing-simulation.md
│   ├── 04-data-exfiltration-simulation.md
│   └── 05-lateral-movement-simulation.md
├── detections/
│   ├── suricata-alerts.md
│   ├── zeek-analysis.md
│   └── investigation-notes.md
├── screenshots/
│   ├── grafana/
│   ├── prometheus/
│   ├── suricata/
│   └── wireshark/
└── scripts/
    ├── log-collection/
    ├── pcap-processing/
    └── maintenance/
```

---

## Documentation Plan

Each major component should have its own documentation file as the repository grows.

### `docs/architecture.md`

Should contain:

- Lab purpose
- Hardware
- VM list
- Network segments
- Data flow
- Security assumptions
- Current limitations

### `docs/prometheus-grafana.md`

Should contain:

- Prometheus installation notes
- node_exporter configuration
- MikroTik or SNMP monitoring notes
- Grafana dashboards
- Screenshots
- Problems encountered and fixes

### `docs/log-collection.md`

Should contain:

- Log sources
- Syslog forwarding
- Linux log paths
- MikroTik log forwarding
- Security logs
- Storage considerations
- Parsing and normalization notes

### `docs/suricata.md`

Should contain:

- Installation notes
- Interface monitoring setup
- `eve.json` output
- Alert examples
- PCAP testing
- Detection limitations

### `docs/zeek.md`

Should contain:

- Installation notes
- Monitored interfaces
- Main Zeek logs
- Example analysis
- Connection, DNS, HTTP, and SSL logs

### `scenarios/*.md`

Each scenario should contain:

- Objective
- Environment
- Tools used
- Expected telemetry
- Actual telemetry
- MITRE ATT&CK mapping
- Screenshots
- Lessons learned
- Detection gaps

---

## Example Scenario Documentation Template

```markdown
# Scenario: Network Scanning

## Objective

Simulate internal network reconnaissance and detect scanning behavior using IDS and network monitoring tools.

## Environment

| Component | Value |
|---|---|
| Attacker host | attacker01 |
| Target host | victim01 |
| Network | Lab network |
| Detection tools | Suricata, Zeek, Wireshark |
| Data sources | PCAP, Suricata alerts, Zeek conn.log |

## Steps

High-level description only:

1. Generate controlled scanning traffic inside the lab.
2. Capture traffic on the monitoring interface.
3. Review Suricata alerts.
4. Analyze Zeek connection logs.
5. Inspect selected packets in Wireshark.
6. Map the activity to MITRE ATT&CK.

## Expected Telemetry

- Multiple connection attempts
- Short-lived TCP sessions
- Increased connection volume
- IDS alerts for scanning behavior
- Zeek connection logs showing many target ports or hosts

## MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Discovery | Network Service Discovery |
| Discovery | Remote System Discovery |

## Detection Result

| Tool | Result |
|---|---|
| Suricata | Detected / Not detected |
| Zeek | Relevant logs generated |
| Wireshark | Manual confirmation possible |
| Prometheus/Grafana | Network activity visible / Not visible |

## Notes

Add investigation notes here.

## Lessons Learned

- What worked
- What did not work
- Detection gaps
- Improvements for next run
```

---

## Current Status

Current project status:

| Area | Status |
|---|---|
| Proxmox VE host | Deployed |
| Linux VMs | In progress |
| Prometheus monitoring | In progress |
| Grafana dashboards | In progress |
| MikroTik monitoring | In progress |
| Syslog forwarding | In progress |
| Suricata | Planned / in progress |
| Zeek | Planned / in progress |
| PCAP collection | Planned  |
| Attack scenarios | Planned |
| Dataset preparation | Planned |
| Bachelor thesis integration | In progress |

This section will be updated as the lab evolves.

---

## Skills Practiced

Technical skills developed through this lab:

- Linux server administration
- Proxmox VE virtualization
- Network segmentation
- SSH administration
- Bash scripting
- Docker basics
- Prometheus monitoring
- Grafana dashboards
- MikroTik monitoring
- Syslog forwarding
- PCAP analysis
- Wireshark usage
- tcpdump usage
- Suricata IDS
- Zeek network security monitoring
- MITRE ATT&CK mapping
- Security event investigation
- Basic incident analysis
- Technical documentation

---

## Roadmap

Short-term:

- Finish stable Proxmox VM layout
- Document network topology
- Add screenshots of Grafana dashboards
- Configure central log collection
- Add Suricata IDS sensor
- Add Zeek network monitoring
- Document first PCAP analysis

Medium-term:

- Create repeatable security simulation scenarios
- Add MITRE ATT&CK mapping for each scenario
- Build initial labeled dataset
- Write basic incident reports for each scenario
- Add anonymization workflow for collected data
- Compare collected dataset structure with public datasets

Long-term:

- Automate parts of data collection
- Improve dashboards
- Add detection rules
- Build a reusable dataset generation methodology
- Integrate findings into bachelor thesis
- Publish sanitized write-ups on GitHub

---

## Security and Ethics

This project is strictly for educational and research purposes.

All testing is performed only in my own isolated lab environment. No third-party systems, public targets, or unauthorized networks are used.

The repository will not contain:

- Credentials
- Private keys
- Sensitive logs
- Real personal data
- Public IP addresses
- Exploit code against real systems
- Unredacted internal network information

All shared screenshots and logs will be sanitized before publication.

---

## Author

Daniel Damek  
Student of Informatics at FIIT STU  
Focus: Linux, cybersecurity, backend development, attack detection, and security monitoring

GitHub: <https://github.com/RajmanCHesus/>
