# Admin Privilege Registry Editor

## Overview
This batch script checks for administrative privileges and re-runs itself with elevated rights if necessary. Once it has administrative privileges, it deletes specific registry values related to legal notices shown at system startup.

## Script Breakdown
1. **Start and Parameters:**
   Sets the parameters for the script execution.
   ```batch
   :start
   set "params=%*"
   ```

2. **Change Directory to Script Location**: Changes the working directory to the script's location.
   ```batch
   cd /d "%~dp0" && (
   ```

3. **Check and Create a VBScript for Elevation**: Checks for administrative privileges and creates a VBScript to elevate if necessary.
   ```batch
if exist "%temp%\getadmin.vbs" del "%temp%\getadmin.vbs"
fsutil dirty query %systemdrive% 1>nul 2>nul || (
    echo Set UAC = CreateObject^("Shell.Application"^) : UAC.ShellExecute "cmd.exe", "/k cd ""%~sdp0"" && %~s0 %params%", "", "runas", 1 >> "%temp%\getadmin.vbs" 
    "%temp%\getadmin.vbs" 
    exit /B
)
   ```

4. **Delete Registry Keys**: Deletes the registry keys for legalnoticecaption and legalnoticetext.
   ```batch
REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v legalnoticecaption /f
REG DELETE "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v legalnoticetext /f
   ```

5. **Exit**: Exits the script.
   ```batch
exit
   ```


## Usage

1. **Run the Script**: Execute the script in an elevated Command Prompt session.
   ```batch
.\admin-privilege-registry-editor.bat
   ```


## License
This script is licensed under the MIT License. See the LICENSE file for details.


