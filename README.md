# TNC-Automator

# Purpose
The TNC-Automator is a specialized diagnostic tool designed to streamline network validation between source and target systems. By automating the native Test-NetConnection (tnc) cmdlet, it replaces manual connectivity checks with a standardized, repeatable process. All findings are aggregated into a timestamped log file, providing IT professionals with a comprehensive audit trail for enterprise troubleshooting and change management.

# Non-technical summary:
This script acts as a digital automated "health check" for network connections between your server and a target destination. Instead of just checking if a server is "on" or "off," it systematically investigates the connection in four logical steps: first, it confirms if the server’s name can be found (DNS); second, it checks if the server is reachable at all (Ping); third, it tests if the specific application service is open for business (Port); and finally, if the connection fails, it automatically maps out the entire network path to pinpoint exactly where the signal is getting blocked. All of these findings are automatically saved into a timestamped text report, allowing you to provide clear evidence to network or security teams without having to manually run multiple complex commands.

# Technical summary:
This script is a modular diagnostic tool designed for automated network validation and troubleshooting within enterprise environments. It follows a structured execution flow that mirrors the OSI model, beginning with local environment baselining via Get-NetIPConfiguration followed by a Layer 7 name resolution check using Resolve-DnsName. The script then validates Layer 3 reachability using ICMP and Layer 4 availability through a targeted TCP port test via Test-NetConnection. A key technical feature is the conditional diagnostic escalation: the script evaluates the TcpTestSucceeded boolean property and only initiates a -TraceRoute if the port test fails, optimizing execution time. To ensure comprehensive audit logging, it utilizes stream redirection (3>&1 2>&1) to capture the Warning and Error streams into a dynamically named, timestamped text file, providing a complete forensic record of connectivity failures and routing hops.



# Features
* **Accuracy:** Eliminates manual data entry errors in log files.
* **Automation:** Replaces 5+ manual commands with one execution.
* **Baseline Capture:** Records local IP and DNS configurations at the start of every test to provide context for the results.  
* **Conditional Escalation:** Automatically initiates a -TraceRoute only if the specific port test fails, saving time during successful checks.
* **Forensic Logging:** Utilizes stream redirection to capture warnings and errors into a timestamped, UTF-8 encoded text file for audit trails.    
* **Layered Diagnostics:** Progressively tests DNS (Layer 7), ICMP Reachability (Layer 3), and TCP Port Availability (Layer 4).  
* **Standardization:** Every team member who uses the script produces the same report format.

# Prerequisites
* **PowerShell Version:** Requires PowerShell 5.1 or newer.  
* **Operating System:** Compatible with Windows 10/11 and Windows Server 2016 or newer.  
* **Permissions:** May require Administrator privileges to create log directories or perform certain network tests.  

# Installation
Download the .txt file (or rename it to .ps1).  

Place it in your desired tools directory (e.g., C:\Scripts\).

# Configuration
Before running, open the script and update the variables in the **Adjustable Settings** section:  
```
$TargetServerName = "192.168.50.20"  # The IP or Hostname to test
$TargetServerPort = '80'             # The destination port
$Dir = "D:\tools\logs\"              # Where to save your reports
```

## Usage
Run the script from a PowerShell terminal:
```powershell
.\TNC-Automator.ps1
```

### Output
The script generates a report named with the format: `YYYYMMDDTHHMMSS_SourceComputer_tnctest_report.txt`.
The report includes:
1.  **Header:** Source/Target details and timestamps.
2.  **Local Config:** Current IP configuration of the source machine.
3.  **DNS Check:** Results from `Resolve-DnsName`.
4.  **Connectivity:** Detailed ICMP and TCP port results.
5.  **Route Analysis:** (Conditional) Full TraceRoute if the port is unreachable.

## Safety Features
The script includes a built-in compatibility check that will `Throw` a terminating error if executed on PowerShell versions 4.0 or older to prevent inaccurate results or command failures.

---
**Author:** Matthew Hinton
