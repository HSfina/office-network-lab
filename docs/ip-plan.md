# IP Addressing Plan — SoufTech Solutions

## Network Overview
- **Company:** SoufTech Solutions
- **Location:** Laayoune, Morocco
- **Author:** Soufiane Hamssassia
- **Date:** 2026-04-22

## VLAN Table

| VLAN | Name       | Network           | Gateway        | DHCP Range                  |
|------|------------|-------------------|----------------|-----------------------------|
| 10   | Management | 192.168.10.0/24   | 192.168.10.1   | 192.168.10.10 - .50 (Static)|
| 20   | Users      | 192.168.20.0/24   | 192.168.20.1   | 192.168.20.10 - .100        |
| 30   | Servers    | 192.168.30.0/24   | 192.168.30.1   | Static only                 |
| 99   | Transit    | 192.168.99.0/30   | -              | Static only                 |

## Device Table

| Device        | VLAN | IP Address      | Method  |
|---------------|------|-----------------|---------|
| SW-core SVI10 | 10   | 192.168.10.1    | Static  |
| SW-core SVI20 | 20   | 192.168.20.1    | Static  |
| SW-core SVI30 | 30   | 192.168.30.1    | Static  |
| SW-core Gi0/1 | 99   | 192.168.99.2    | Static  |
| R1 Gi0/0      | 99   | 192.168.99.1    | Static  |
| PC_MGMT       | 10   | 192.168.10.10   | Static  |
| Server        | 30   | 192.168.30.10   | Static  |
| PC_Server     | 30   | 192.168.30.11   | Static  |
| Printer_MGMT  | 10   | DHCP            | Dynamic |
| PC_user1      | 20   | DHCP            | Dynamic |
| PC_user2      | 20   | DHCP            | Dynamic |

## DNS Records

| Hostname                  | Type | IP            |
|---------------------------|------|---------------|
| server.souftech.local     | A    | 192.168.30.10 |
| gateway.souftech.local    | A    | 192.168.10.1  |
| pc-mgmt.souftech.local    | A    | 192.168.10.10 |

## Security Policy

### Port Security Configuration

| Switch | VLAN | Mode | Max MAC | Violation |
|--------|------|------|---------|-----------|
| SW1 | Management (10) | Strict | 1 | Shutdown |
| SW2 | Users (20) | Relaxed | 3 | Restrict |
| SW3 | Servers (30) | Strict | 1 | Shutdown |

### Design Decision
- SW1 & SW3: Strict mode because management and server 
  ports must never allow unauthorized devices.
- SW2: Restrict mode because user ports need flexibility 
  (laptop + IP phone), without hard shutdowns.
  
### Unused Ports
All unused access ports are administratively shutdown
to prevent unauthorized access.