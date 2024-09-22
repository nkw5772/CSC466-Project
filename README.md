# CSC466-Project

## For windows:
   We will use the flare vm or a standard windows machine in order to execute malware.

   ### Instructions:
      Build out a new virtual machine with a windows iso file, this will be used for the flare VM
      Complete the steps as prompt, ensure you do not perform updates (It will disable updates if you don’t)
      
      After, you will need to disable windows defender.
      You can go into group policy and disable all of antivirus, as well as download a script to delete windows defender [here](https://github.com/ionuttbara/windows-defender-remover).

      Install the script for FlareVM [here](https://github.com/mandiant/flare-vm).
      Use the commands:
      1. (New-Object net.webclient).DownloadFile('https://raw.githubusercontent.com/mandiant/flare-vm/main/install.ps1',"$([Environment]::GetFolderPath("Desktop"))\install.ps1")
      2. Unblock-File .\install.ps1
      3. Set-ExecutionPolicy Unrestricted -Force
         Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force (if you have any issues)
      4. ./install ps1

## Analysis
Down the necessary malware from malware bazaar (database for download)

Malware analysis:

Analyzing c59da5938f667c04ca2ba3639b6cb3d55813fc189d4b2f412613b4bfa36ae:

Tools used: Dependency Walker, PE-Studio, X32dbg, Task Manager, procmon

PE Studio analysis:

Import libraries such as ole32.dll, kernel32.dll, user32.dll. Flag showing that there are missing import/export entries indicates that the binary may be obfuscated, using the sleep timer to delay loading for evasion

We see functions such as VirtualAlloc, GetKeyboardType that indicate keylogging, memory allocation, and other injection techniques.

You can see T1055 Process Injection and T1010 Window Discovery, potential attempts for interacting with processes, injecting malicious code. Commonly seen with RATs, password stealers, and keylogging.

We find delphi signatures (used for packing)

Dependency Walker:

Has missing dependencies which may be packed or obfuscated and APIs are resolved dynamically during runtime.
