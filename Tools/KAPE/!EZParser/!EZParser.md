# [!EZParser](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/!EZParser.mkape)

## [AmcacheParser](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/ProgramExecution/AmcacheParser.mkape)

### Formats available in Module

- CSV

### Command

- amcacheparser.exe -f %sourceFile% --csv %destinationDirectory% -i

	- Parses the source file (FileMask: Amcache.hve) and outputs a CSV (or other specified format) to the user-specified destination directory

## [AppCompatCacheParser](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/ProgramExecution/AppCompatCacheParser.mkape)

### Formats available in Module

- CSV

### Command

- appcompatcacheparser.exe -d %sourceDirectory% --csv %destinationDirectory%

	- Parses the source file (FileMask: SYSTEM) and outputs a CSV (or other specified format) to the user-specified destination directory

## [RecentFileCacheParser](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/ProgramExecution/RecentFileCacheParser.mkape)

### Formats available in Module

- CSV/JSON

### Command

- recentfilecacheparser.exe -f %sourceFile% --json %destinationDirectory%

	- Parses the source file (FileMask: RecentFileCache.bcf) and outputs a CSV (or other specified format) to the user-specified destination directory

## [RECmd_Kroll](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Registry/RECmd_Kroll.mkape)

### Formats available in Module

- CSV

### Command

- recmd.exe -d %sourceDirectory% --bn BatchExamples\Kroll_Batch.reb --nl false --csv %destinationDirectory% -q

	- Parses the source directory (recursively) using the Kroll Batch File, replays Registry transaction logs, and outputs a CSV (or other specified format) to the user-specified destination directory

## [SBECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileFolderAccess/SBECmd.mkape)

### Formats available in Module

- CSV

### Command

- sbecmd.exe -d %sourceDirectory% --csv %destinationDirectory% -q

	- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [WxTCmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileFolderAccess/WxTCmd.mkape)

### Formats available in Module

- CSV

### Command

- wxtcmd.exe -f %sourceFile% --csv %destinationDirectory%

	- Parses the source file (FileMask: ActivitiesCache.db) and outputs a CSV (or other specified format) to the user-specified destination directory

## [EvtxECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/EventLogs/EvtxECmd.mkape)

### Formats available in Module

- CSV/XML/JSON

### Command

- evtxecmd.exe -d %sourceDirectory% --csv %destinationDirectory%

	- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [RBCmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Misc/RBCmd.mkape)

### Formats available in Module

- CSV

### wxtcmd.exe -d %sourceDirectory% --csv %destinationDirectory%

- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [PECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/ProgramExecution/PECmd.mkape)

### Formats available in Module

- CSV/HTML/JSON

### Command

- mftecmd.exe -d %sourceDirectory% --csv %destinationDirectory% -q

	- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [MFTECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileSystem/MFTECmd.mkape)

### Formats available in Module

- CSV/JSON

### Modules

- [$Boot](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileSystem/MFTECmd_%24Boot.mkape)

	- mftecmd.exe -f %sourceFile% --csv %destinationDirectory%

		- Parses the source file (FileMask: $Boot) and outputs a CSV (or other specified format) to the user-specified destination directory

- [$J](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileSystem/MFTECmd_%24J.mkape)

	- mftecmd.exe -f %sourceFile% --csv %destinationDirectory%

		- Parses the source file (FileMask: $J) and outputs a CSV (or other specified format) to the user-specified destination directory

- [$MFT](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileSystem/MFTECmd_%24MFT.mkape)

	- mftecmd.exe -f %sourceFile% --csv %destinationDirectory%

		- Parses the source file (FileMask: $MFT) and outputs a CSV (or other specified format) to the user-specified destination directory

- [$SDS](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileSystem/MFTECmd_%24SDS.mkape)

	- mftecmd.exe -f %sourceFile% --csv %destinationDirectory%

		- Parses the source file (FileMask: $SDS) and outputs a CSV (or other specified format) to the user-specified destination directory

## [LECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileFolderAccess/LECmd.mkape)

### Formats available in Module

- CSV/HTML/JSON

### Command

- lecmd.exe -d %sourceDirectory% --csv %destinationDirectory% -q

	- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [JLECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/FileFolderAccess/JLECmd.mkape)

### Formats available in Module

- CSV/HTML/JSON

### Command

- jlecmd.exe -d %sourceDirectory% --csv %destinationDirectory% -q

	- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [SQLECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Misc/SQLECmd.mkape)

### Formats available in Module

- CSV/JSON

### sqlecmd.exe -d %sourceDirectory% --csv %destinationDirectory%

- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory

## [SrumECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Misc/SrumECmd.mkape)

### Formats available in Module

- CSV

### sumecmd.exe -d %sourceDirectory% -k --csv %destinationDirectory%

- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory
- Be sure to check your console log and see if the SRUDB.dat needs to be repaired with ESENTUTL prior to being successfully parsed by SrumECmd!

## [SumECmd](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Misc/SumECmd.mkape)

### Formats available in Module

- CSV

### sumecmd.exe -d %sourceDirectory%\Windows\System32\LogFiles\SUM --csv %destinationDirectory%

- Parses the source directory (recursively) and outputs a CSV (or other specified format) to the user-specified destination directory
- Be sure to check your console log and see if the SUM DB files need to be repaired with ESENTUTL prior to being successfully parsed by SumECmd!

