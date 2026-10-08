# Network Attack Detection & Reporting

## Project Overview

This project was completed as part of **Assignment 1 of the Bincom Academy Cybersecurity Apprenticeship Program**.

The objective was to build a controlled virtual lab for detecting and analyzing network activity using a **Cowrie SSH Honeypot**, **Nmap**, and **Wireshark**.

The lab simulated network reconnaissance and SSH connection activity from a Kali Linux machine against a Cowrie honeypot hosted on Ubuntu.

## Lab Environment

| Component           | Details             |
| ------------------- | ------------------- |
| Attacker/Testing VM | Kali Linux          |
| Honeypot VM         | Ubuntu Linux        |
| Kali IP             | `10.0.2.11`         |
| Ubuntu IP           | `10.0.2.15`         |
| Honeypot            | Cowrie SSH Honeypot |
| SSH Port            | `2222/tcp`          |
| Network             | VirtualBox NAT      |

## Tools Used

* Kali Linux
* Ubuntu Linux
* Cowrie SSH Honeypot
* Nmap
* Wireshark
* OpenSSH

## Methodology

The project followed these main stages:

1. Deployed Cowrie on an Ubuntu virtual machine.
2. Configured Cowrie to listen for SSH connections on TCP port `2222`.
3. Verified connectivity between Kali Linux and Ubuntu.
4. Used Nmap to perform service discovery against the Ubuntu host.
5. Connected from Kali to the Cowrie SSH service.
6. Captured the network traffic using Wireshark.
7. Analyzed Cowrie logs and session telemetry.
8. Correlated network traffic with the corresponding Cowrie session.
9. Mapped observed activity to relevant MITRE ATT&CK techniques.

## Nmap Results

The service scan identified TCP port `2222` as open and associated it with an SSH service.

```text
2222/tcp open ssh
OpenSSH 9.2p1 Debian 2+deb12u3
```

The SSH service identity shown by Nmap represents the **Cowrie-emulated SSH service**.

## Wireshark Analysis

The captured traffic was filtered using:

```text
ip.addr == 10.0.2.15 && tcp.port == 2222
```

The capture contained the TCP connection establishment, SSH banners, and SSH key-exchange traffic.

After the SSH key exchange, the session traffic became encrypted, so packet inspection was limited to the observable connection metadata and handshake information.

## Cowrie Telemetry

Cowrie recorded activity from the Kali Linux host, including:

* Source IP: `10.0.2.11`
* Source port: `40294`
* Destination: `10.0.2.15:2222`
* SSH session ID: `63f7a45ac1bf`
* HASSH: `eeca2460550b9ded084ecf2f70a75356`

The Cowrie session lasted approximately 123 seconds and closed after timeout. No successful login was recorded for the Kali-originated session.

A separate local test session was also performed to verify Cowrie's simulated SSH environment and generated a TTY recording.

## MITRE ATT&CK Mapping

The observed activity was mapped to the following techniques:

| Technique | Description                  |
| --------- | ---------------------------- |
| T1046     | Network Service Scanning     |
| T1021.004 | SSH                          |
| T1033     | System Owner/User Discovery  |
| T1083     | File and Directory Discovery |

These mappings represent activity performed within the controlled lab environment.

## Key Findings

* Cowrie successfully operated as an SSH honeypot.
* Nmap identified the exposed SSH service on TCP port `2222`.
* Wireshark captured the network connection and SSH negotiation.
* Cowrie provided additional host-level session telemetry.
* Network traffic and Cowrie logs could be correlated using the source IP, source port, destination port, and session information.
* The combination of network and host telemetry provided a clearer view of the simulated activity.

## Skills Demonstrated

* Honeypot deployment and configuration
* Network reconnaissance analysis
* Nmap service enumeration
* Wireshark packet analysis
* SSH traffic analysis
* Log analysis
* Security event correlation
* Basic MITRE ATT&CK mapping
* Incident reporting
* Security monitoring

## Evidence

The repository contains:

* Assignment report
* Lab evidence screenshots
* Wireshark packet capture
* Supporting documentation

## Security Considerations

This project was performed entirely within an isolated VirtualBox lab using systems controlled by the author.

No external systems or third-party networks were targeted.

## Author

**Akinrelere Oluwatobi**

Cybersecurity Student | SOC / Blue Team

[LinkedIn](https://linkedin.com/in/oluwatobi-akinrelere-8298b03b2)
