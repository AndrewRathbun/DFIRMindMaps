# Cobalt Strike Built-in Lateral Movement Capabilities
Based on CONTI Leak

## jump Moduke

### winrm

- Source Host

	- Microsoft-Windows-WinRM

		- EID 6 Creating WSMan Session

			- Process ID that initiated the connection

		- EID 31 WSMan Session Created Successfully

	- Microsoft-Windows-Sysmon

		- EID 3 Network Connection

			- Process Name: powershell.exe
			- Network Direction :  egress
			- Destination Port : 5685

- Destination Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wsmprovhost.exe

				- This binary is the host process for WinRM plugins
				- If the command invokes a native cmdlet, it executes directly within the context of wsmprovhost.exe

			- Process Command Line : C:\Windows\system32\wsmprovhost.exe -Embedding
			- Parent Process Name: svchost.exe
			- Parent Command Line : C:\Windows\system32\svchost.exe -k DcomLaunch

		- EID 1 Process Creation

			- Process Name : powershell.exe

				- CobaltStrike's winrm lateral movement command is different from winrm64 in that wsmprovhost.exe executes commands in an invoked powershell.exe instance context
				- Interacting with compromised systems via shell commands creates the following process tree

					- wsmprovhost.exe

						- powershell.exe

							- cmd.exe

			- Process Command Line: "c:\windows\syswow64\windowspowershell\v1.0\powershell.exe" -Version 5.1 -s -NoLogo -NoProfile
			- Parent Process Name: wsmprovhost.exe
			- Parent Command Line :  C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : \lsass

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 3 Network Connection

			- User : NT AUTHORITY\SYSTEM
			- Source IP
			- Destination IP : [HOST IP]
			- Process Name: System
			- Network Direction :  ingress
			- Destination Port : 5685

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : \PSHost.[%NUMBERS%].[%PID%].DefaultAppDomain.wsmprovhost

		- EID 11 File Created

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 23 File Deleted

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 22 DNS query

			- Query Name : [Accessed Host Name]
			- Query Result : [IP Address of the accessed host]
			- Process Name : wsmprovhost.exe

	- Microsoft-Windows-Security-Auditing

		- EID 4656 A handle to an object was requested

			- Object Server : WS-Management Listener
			- Process Name : C:\Windows\System32\svchost.exe

	- PowerShell

		- EID 400 Engine state is changed from None to Available

			- Start of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

			- Host Application : C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 403 Engine state is changed from Available to Stopped

			- Completion of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

	- Microsoft-Windows-Powershell

		- EID 4104 Script Block Logging

			- Verbose
			- Script blocks exceeding the maximum length of an event log message are fragmented into multiple entries.

				- [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((func_get_proc_address kernel32.dll VirtualAlloc)
				- To parse the script out of multiple events we can use https://github.com/matthewdunwoody/block-parser

			- This event doesn't record the output of the script
			- Executed script properties

				- The script first decode the Base64 encoded payload 
				- The script uses the .Net API to call Windows API function in memory using assemblies.
				- The script allocates some memory
				- Copy the payload in the allocated memory space
				- Execute the payload

					- During our tests, the payload is a 64-bits DLL file
					- The payload strings contain :
- "beacon.dll"
- "beacon.x86.dll"
- "beacon.x64.dll"

		- EID 4103 Module Logging

			- Verbose: generates a large volume of events
			- Records command outputs

	- Microsoft-Windows-WinRM

		- EID 91 Creating WSMan shell on server 
		- EID 31 WSMan Session Created Successfully
		- EID 142 WSMan operation CreateShell failed

			- Helpful when WinRM is not enabled on the target host

		- EID 169 User Authenticated Successfully

			- The user who was connected via remoting

		- EID 81 Processing Client Request for Operation CreateShell

			- Start of remoting activity

		- EID 134 Sending Response for Operation DeleteShell

			- End of Remoting Activity

### winrm64 

- Source Host

	- Microsoft-Windows-WinRM

		- EID 6 Creating WSMan Session

			- Process ID that initiated the connection

		- EID 31 WSMan Session Created Successfully

	- Microsoft-Windows-Sysmon

		- EID 3 Network Connection

			- Process Name: powershell.exe
			- Network Direction :  egress
			- Destination Port : 5685

- Destination Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wsmprovhost.exe

				- This binary is the host process for WinRM plugins
				- If the command invokes a native cmdlet, it executes directly within the context of wsmprovhost.exe

			- Process Command Line : C:\Windows\system32\wsmprovhost.exe -Embedding
			- Parent Process Name: svchost.exe
			- Parent Command Line : C:\Windows\system32\svchost.exe -k DcomLaunch

		- EID 1 Process Creation

			- Process Name : cmd.exe

				- Interacting with compromised systems via winrm64 lateral movement technique with shell command invokes CMD.EXE and WSMPROVHOST.EXE as parent.

			- Parent Process Name: wsmprovhost.exe
			- Parent Command Line :  C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : \lsass

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 3 Network Connection

			- User : NT AUTHORITY\SYSTEM
			- Source IP
			- Destination IP : [HOST IP]
			- Process Name: System
			- Network Direction :  ingress
			- Destination Port : 5685

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : \PSHost.[%NUMBERS%].[%PID%].DefaultAppDomain.wsmprovhost

		- EID 11 File Created

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 23 File Deleted

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 22 DNS query

			- Query Name : [Accessed Host Name]
			- Query Result : [IP Address of the accessed host]
			- Process Name : wsmprovhost.exe

	- Microsoft-Windows-Security-Auditing

		- EID 4656 A handle to an object was requested

			- Object Server : WS-Management Listener
			- Process Name : C:\Windows\System32\svchost.exe

	- PowerShell

		- EID 400 Engine state is changed from None to Available

			- Start of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

			- Host Application : C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 403 Engine state is changed from Available to Stopped

			- Completion of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

	- Microsoft-Windows-Powershell

		- EID 4104 Script Block Logging

			- Verbose
			- Script blocks exceeding the maximum length of an event log message are fragmented into multiple entries.

				- [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((func_get_proc_address kernel32.dll VirtualAlloc)
				- To parse the script out of multiple events we can use https://github.com/matthewdunwoody/block-parser

			- This event doesn't record the output of the script
			- Executed script properties

				- The script first decode the Base64 encoded payload 
				- The script uses the .Net API to call Windows API function in memory using assemblies.
				- The script allocates some memory
				- Copy the payload in the allocated memory space
				- Execute the payload

					- During our tests, the payload is a 64-bits DLL file
					- The lateral movement uses reflective dll loading technique
					- The payload strings contain :
- "beacon.dll"
- "beacon.x86.dll"
- "beacon.x64.dll"

		- EID 4103 Module Logging

			- Verbose: generates a large volume of events
			- Records command outputs

	- Microsoft-Windows-WinRM

		- EID 91 Creating WSMan shell on server 
		- EID 31 WSMan Session Created Successfully
		- EID 142 WSMan operation CreateShell failed

			- Helpful when WinRM is not enabled on the target host

		- EID 169 User Authenticated Successfully

			- The user who was connected via remoting

		- EID 81 Processing Client Request for Operation CreateShell

			- Start of remoting activity

		- EID 134 Sending Response for Operation DeleteShell

			- End of Remoting Activity

### psexec & psexec64

- Destination Host

	- Microsoft-Windows-Security-Auditing

		- EID 5145 A network share object was checked to see whether client can be granted desired access

			- Relative Target Name : svcctl
			- Share Name : \\*\IPC$

		- EID 4697 A service was installed in the system

			- Service File Name: \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe

	- System

		- EID 7045 New Service was Installed

			- Service File Name: \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Command Line : \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe
			- Parent Command Line : C:\Windows\System32\services.exe

		- EID 1 Process Creation

			- Command Line : C:\Windows\System32\rundll32.exe
			- Arguments count : 0

				- This method's default behavior spawn rundll32 with no argument

			- Parent Image : \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe

		- EID 13 Registry Value Set

			- Image Path : \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe

- Zeek

	- DCE-RPC

		- Endpoint

			- svcctl

		- Operation

			- CreateService*

### psexec_psh

- Destination Host

	- Microsoft-Windows-Security-Auditing

		- EID 5145 A network share object was checked to see whether client can be granted desired access

			- Relative Target Name : status_[0-9a-f]{2}
			- Share Name : \\*\IPC$

		- EID 4697 A service was installed in the system

			- Service File Name contains : %COMSPEC% or powershell

	- System

		- EID 7045 New Service was Installed

			- Service File Name contains : %COMSPEC% or powershell

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Command Line Arguments : powershell, -nop, hidden, -encodedcommand
			- Process Name : powershell.exe

				- Interacting with the beacon via the CS shell command would invoke a cmd.exe instance.

			- Parent Process Name : cmd.exe

		- EID 17 Pipe Created

			- Command Line : \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe
			- Parent Command Line : C:\Windows\System32\services.exe

		- EID 18 Pipe Connected

			- Image Path : \\127.0.0.1\ADMIN$\[SERVICE_RANDOM_NAME].exe

## remote-exec

### wmi

- Destination Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wmiprvse.exe
			- Process Command Line : C:\Windows\system32\wbem\wmiprvse.exe -secured -Embedding or C:\Windows\system32\wbem\wmiprvse.exe -Embedding
			- Parent Process Name : svchost.exe
			- Parent Process Command Line : C:\Windows\system32\svchost.exe -k DcomLaunch

		- EID 1 Process Creation

			- Parent Process Name: wmiprvse.exe
			- LogonID : Is not 0x3E7

				- Not a LocalSystem account

		- EID 3 Network Connection

			- Network Direction : ingress
			- Image : C:\Windows\system32\svchost.exe
			- Source port : >= 49152
			- Source IP : is not 127.0.0.1 and not ::1

- Zeek

	- DCE-RPC

		- Endpoint

			- IWbemLevel1Login

				- During protocol initialization, The client MUST call the IWbemLevel1Login::NTLMLogin method.

			- IWbemServices

				- Web-based Enterprise Management Services

		- Operation

			- NTLMLogin

				- Protocol Initialization

			- ExecMethod

				- Executes a CIM method that is implemented by a CIM class or a CIM instance

			- GetObject

				- Retrieves a CIM class or a CIM instance

### winrm

- Source Host

	- Microsoft-Windows-WinRM

		- EID 6 Creating WSMan Session

			- Process ID that initiated the connection

		- EID 31 WSMan Session Created Successfully

	- Microsoft-Windows-Sysmon

		- EID 3 Network Connection

			- Process Name: powershell.exe
			- Network Direction :  egress
			- Destination Port : 5685

- Destination Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wsmprovhost.exe

				- This binary is the host process for WinRM plugins
				- If the command invokes a native cmdlet, it executes directly within the context of wsmprovhost.exe

			- Process Command Line : C:\Windows\system32\wsmprovhost.exe -Embedding
			- Parent Process Name: svchost.exe
			- Parent Command Line : C:\Windows\system32\svchost.exe -k DcomLaunch

		- EID 1 Process Creation

			- Process Name : cmd.exe

				- Interacting with compromised systems via winrm64 lateral movement technique with shell command invokes CMD.EXE and WSMPROVHOST.EXE as parent.

			- Parent Process Name: wsmprovhost.exe
			- Parent Command Line :  C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : \lsass

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 18 Pipe Connected

			- Process Name : wsmprovhost.exe
			- Pipe Name : ANONYMOUS PIPE

		- EID 3 Network Connection

			- User : NT AUTHORITY\SYSTEM
			- Source IP
			- Destination IP : [HOST IP]
			- Process Name: System
			- Network Direction :  ingress
			- Destination Port : 5685

		- EID 17 Pipe Created

			- Process Name : wsmprovhost.exe
			- Pipe Name : \PSHost.[%NUMBERS%].[%PID%].DefaultAppDomain.wsmprovhost

		- EID 11 File Created

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 23 File Deleted

			- File Directory : C:\Users\[USERNAME]\AppData\Local\Temp
			- File Extension : ps1 OR psm1
			- Process Name : wsmprovhost.exe

		- EID 22 DNS query

			- Query Name : [Accessed Host Name]
			- Query Result : [IP Address of the accessed host]
			- Process Name : wsmprovhost.exe

	- Microsoft-Windows-Security-Auditing

		- EID 4656 A handle to an object was requested

			- Object Server : WS-Management Listener
			- Process Name : C:\Windows\System32\svchost.exe

	- PowerShell

		- EID 400 Engine state is changed from None to Available

			- Start of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

			- Host Application : C:\Windows\system32\wsmprovhost.exe -Embedding

		- EID 403 Engine state is changed from Available to Stopped

			- Completion of a PowerShell session
			- Host Name = ServerRemoteHost

				- if HostName = ConsoleHost it means that it was executed locally otherwise ServerRemoteHost for remotely

			- Engine Version

				- Can be good indication of downgraded attacks using old versions of powershell like v2.0

	- Microsoft-Windows-WinRM

		- EID 91 Creating WSMan shell on server 
		- EID 31 WSMan Session Created Successfully
		- EID 142 WSMan operation CreateShell failed

			- Helpful when WinRM is not enabled on the target host

		- EID 169 User Authenticated Successfully

			- The user who was connected via remoting

		- EID 81 Processing Client Request for Operation CreateShell

			- Start of remoting activity

		- EID 134 Sending Response for Operation DeleteShell

			- End of Remoting Activity

### psexec

- Zeek

	- DCE-RPC

		- Endpoint

			- svcctl

		- Operation

			- CreateService*

- Destination Host

	- Microsoft-Windows-Security-Auditing

		- EID 5145 A network share object was checked to see whether client can be granted desired access

			- Relative Target Name : svcctl
			- Share Name : \\*\IPC$

		- EID 4697 A service was installed in the system

			- Creates and start a service remotely with random Service Name and the passed on command as Service File Name.

	- System

		- EID 7045 New Service was Installed

			- Creates and start a service remotely with random Service Name and the passed on command as Service File Name.

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- The main difference between this feature and jump psexec or jump psexec64 is that remote-exec psexec does not generate a service executable and upload it to the target.
			- Process Name : cmd.exe or powershell.exe

				- Look for services.exe child process for malicious behavior like spawning system shells cmd.exe and powershell.exe or other discovery binaries

			- Parent Command Line : C:\Windows\System32\services.exe

## shell

### wmic

- Source Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wmic.exe 
			- Process Arguments : /node, process, call, and create

				- Arguments process, call, and create are mandatory for invoking executing command via wmic.exe
				- For remote command execution the argument /node must be used

		- EID 3 Network Connection

			- Network Direction : egress 
			- Image : C:\Windows\system32\wbem\wmic.exe
			- Source port : >= 49152
			- Source IP : is not 127.0.0.1 and not ::1

	- Microsoft-Windows-Security-Auditing

		- EID 4648 A logon was attempted using explicit credentials 

			- Additional Information : RPCSS/* 

				- The RPCSS service is the Service Control Manager for COM and DCOM servers. It performs object activations requests, object exporter resolutions and distributed garbage collection for COM and DCOM servers.

			- Process Name : C:\Windows\System32\svchost.exe

		- EID 4648 A logon was attempted using explicit credentials 

			- Additional Information : host/* 
			- Process Name : C:\Windows\System32\wbem\wmic.exe

				- HOST service can also be used for remotely executing commands on the target system via WMI

- Destination Host

	- Microsoft-Windows-Sysmon

		- EID 1 Process Creation

			- Process Name : wmiprvse.exe
			- Process Command Line : C:\Windows\system32\wbem\wmiprvse.exe -secured -Embedding or C:\Windows\system32\wbem\wmiprvse.exe -Embedding
			- Parent Process Name : svchost.exe
			- Parent Process Command Line : C:\Windows\system32\svchost.exe -k DcomLaunch

		- EID 1 Process Creation

			- Parent Process Name: wmiprvse.exe
			- LogonID : Is not 0x3E7

				- Not a LocalSystem account

		- EID 3 Network Connection

			- Network Direction : ingress
			- Image : C:\Windows\system32\svchost.exe
			- Source port : >= 49152
			- Source IP : is not 127.0.0.1 and not ::1

- Zeek

	- DCE-RPC

		- Endpoint

			- IWbemLevel1Login

				- During protocol initialization, The client MUST call the IWbemLevel1Login::NTLMLogin method.

			- IWbemServices

				- Web-based Enterprise Management Services

		- Operation

			- NTLMLogin

				- Protocol Initialization

			- ExecMethod

				- Executes a CIM method that is implemented by a CIM class or a CIM instance

			- GetObject

				- Retrieves a CIM class or a CIM instance

## Blogposts

### [Detecting CONTI CobaltStrike Lateral Movement Techniques - Part 1](https://www.unh4ck.com/detection-engineering-and-threat-hunting/lateral-movement/detecting-conti-cobaltstrike-lateral-movement-techniques-part-1)

### [Detecting CONTI CobaltStrike Lateral Movement Techniques - Part 2](https://www.unh4ck.com/detection-engineering-and-threat-hunting/lateral-movement/detecting-conti-cobaltstrike-lateral-movement-techniques-part-2)

