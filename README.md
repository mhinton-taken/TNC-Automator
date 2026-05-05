# TNC-Automator
This PowerShell tool automates network validation using the tnc (Test-NetConnection) cmdlet. It sequentially tests DNS, ICMP, and TCP port status. It features conditional escalation, running a TraceRoute only if port tests fail, and captures all results in a timestamped forensic log for enterprise troubleshooting.It follows a structured execution flow that mirrors the OSI model to identify exactly where a network connection is failing.  

# Features
* **Layered Diagnostics:** Progressively tests DNS (Layer 7), ICMP Reachability (Layer 3), and TCP Port Availability (Layer 4).  
* **Conditional Escalation:** Automatically initiates a -TraceRoute only if the specific port test fails, saving time during successful checks.  
* **Forensic Logging:** Utilizes stream redirection to capture warnings and errors into a timestamped, UTF-8 encoded text file for audit trails.  
* **Baseline Capture:** Records local IP and DNS configurations at the start of every test to provide context for the results.  

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
