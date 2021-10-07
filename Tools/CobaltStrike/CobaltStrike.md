# Cobalt Strike

## Network indicators

### IP Address 
       or Hostname

- Threat Intelligence lookups

	- AlienVault OTX
	- VirusTotal lookup
	- RiskIQ
	- Cobaltstrikebot (only for IP)

- Manual analysis

	- Shodan

		- Known Cobalt Strike port
(port:50050)
		- Default certificate usage
(ssl.cert.serial:146473198)
		- Other methods include:
JARM fingerprinting, HTTP header analysis

## Executable

### Automatic extraction 

- Triage sandbox by Hatching
- CobaltStrikeParser by SentinelOne
- 1786 by Didier Stevens

### Manual extraction

- Decrypt shellcode 

	- Decode Base64 (if applicable)
	- Decompress GZIP (if applicable)
	- Decode XOR (if applicable)

- Emulate shellcode

	- SCDBG
	- Speakeasy

## Live analysis

### Powershell

-                Event ID: 400/403/600
- Event ID:4104(only when script block logging is available)

### Analyse running processes

- Execution of Rundll32.exe without arguments 
(default for Cobalt Strike)
- Look for suspicious parent-child process relations
(e.g. Winword.exe spawning cmd.exe)
- Scan running processes with 'CobaltStrikeScan' by Apr4h 

### Analyse service creation events

- Windows Event log:
- 4697 (security event log)
- 7045 (system event log)
- 7034 (system event log)

## Memory analysis

### Full memory dump

- Volatility -f memory.image 

	- Plugins: 
- windows.psscan
- windows.pstree
- windows.cmdscan
- windows.netscan
- windows.malfind
	- Plugin:
- 'cobaltstrikescan/cobaltstrikeconfig' 
by JPCERTCC

### Process dump

- CobaltstrikeParser -process.dmp

