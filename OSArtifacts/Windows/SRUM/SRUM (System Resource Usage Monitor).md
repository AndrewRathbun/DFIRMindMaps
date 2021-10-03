# SRUM (System Resource Usage Monitor)

## Repairing and parsing the SRUM DB (SRUDB.dat) 

### Repair

- 1. Make a copy of the C:\Windows\System32\SRU directory

	- You will be modifying the SRUDB.dat during this repair process!

- 2. Ensure the SRU directory isn't Read Only

	- Right click on SRU directory and remove Read Only and apply to all subfolders and files

- 3. Open PowerShell as Admin and run the following commands to repair the SRUDB.dat

	- esentutl.exe /r sru /i
	- esentutl.exe /p SRUDB.dat

### Parse

- [SrumECmd by Eric Zimmerman](https://github.com/EricZimmerman/Srum)

	- srumecmd.exe -d "path\to\repaired\DB" --csv "path\to\output" --debug

		- --debug is optional, but more verbose logging is helpful for troubleshooting and general awareness of what the tool is doing
		- Provide the SOFTWARE hive in your "path\to\repaired\DB" to obtain additional output related to Performance Data

- [ESEDatabaseView by NirSoft](https://www.nirsoft.net/utils/ese_database_view.html)

	- .\ESEDatabaseView.exe /table "path\to\repaired\DB" * /scomma "path\to\output\SRUM_*.csv"

		- * serves as a wildcard so the CSV filenames will be SRUM_TableName.csv

- [SRUM-Dump by Mark Baggett](https://github.com/MarkBaggett/srum-dump)

	- Provides GUI to parse SRUDB.dat

## Use Cases

### Incident Response

- Identifying data exfil using Bytes Sent/Bytes Received
- Tracking anomalous I/O activity by a specific binary

### Law Enforcement

- Track I/O activity by suspects using applications to commit crimes (IP Theft, CSAM cases, etc)
- Track network activity by suspects using applications to commit crimes (IP Theft, CSAM cases, etc)

### Corporate

- Insider Threat

	- Employee stealing secrets and sharing them with a competitor

## Artifacts of Interest

### Processes Run

- AppID
- User executing app
- App energy usage
- Bytes sent
- Bytes received

### App Push Notifications

- AppID
- User
- Payload size

### Networking Activity

- Network interface
- Network name
- Bytes sent
- Bytes received
- Connection time
- Connection duration

### Energy Usage

- Charge capacity
- Charge level
- Time

## Registry

### Location

- SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions

## SRUDB.dat

### Historical Data

- Stores 30 days of application data

### Location

- C:\Windows\System32\SRU\SRUDB.dat

## Important Considerations

### Records to SRUDB.dat approximately every 60 minutes or upon clean system shutdown

## Tables

### {DA73FB89-2BEA-4DDC-86B8-6E048C6DA477}

### {FEE4E14F-02A9-4550-B5CE-5FA2DA202E37}LT

### {FEE4E14F-02A9-4550-B5CE-5FA2DA202E37}

### {D10CA2FE-6FCF-4F6D-848E-B2E99266FA89}

- AppResourceUseInfo

### {5C8CF1C7-7257-4F13-B223-970EF5939312}

### {7ACBBAA3-D029-4BE4-9A7A-0885927F1D8F}

### {DD6636C4-8929-4683-974E-22C046A43763}

- NetworkConnection

### {D10CA2FE-6FCF-4F6D-848E-B2E99266FA86}

- PushNotification

### {973F5D5C-1D90-4944-BE8E-24B94231A174}

- NetworkUsage

### SruDbCheckpointTable

### SruDbIdMapTable

### MSysLocales

### MSysObjids

### MSysObjectsShadow

### MSysObjects

