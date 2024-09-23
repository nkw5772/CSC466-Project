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

   Download the necessary malware from malware bazaar (database for download)

## First Analysis

   Analyzing c59da5938f667c04ca2ba3639b6cb3d55813fc189d4b2f412613b4bfa36ae:

   Tools used: Dependency Walker, PE-Studio, X32dbg, Task Manager, procmon

   ### PE Studio analysis:

   Import libraries such as ole32.dll, kernel32.dll, user32.dll. Flag showing that there are missing import/export entries   
   indicates that the binary may be obfuscated, using the sleep timer to delay loading for evasion

   We see functions such as VirtualAlloc, GetKeyboardType that indicate keylogging, memory allocation, and other injection 
   techniques.

   You can see T1055 Process Injection and T1010 Window Discovery, potential attempts for interacting with processes, 
   injecting malicious code. Commonly seen with RATs, password stealers, and keylogging.

   We find delphi signatures (used for packing)

   ### Dependency Walker:

   Has missing dependencies which may be packed or obfuscated and APIs are resolved dynamically during runtime.

   ### X32dbg:

   Multiple DLLs being loaded into the process, ntdll.dll is called upon, commonly used to evade higher-level monitoring 
   tools

   Memory map shows potential dynamic allocation of memory to inject or modify code

   A lot shows that it is performing process injection, using ntdll.dll.

   ### Summary:

   Shows signs of being packed/obfuscated

   Duplicate imports

   Delphi signatures found, has a packer tool

   Process injection is found with ntdll.dll and LdrInitializeProcess indicate code injection

   Keylogging functions found

   Reaches system breakpoint (used for anti-debugging)

   Shows registry key modifications

## Second Analysis

   Malware analysis on 41853d9b1ea1a9fbc492589d25aa6f515ca0ad241ce844af76c55a795873ed9

   ### PEStudio:

   Imports files such as user32.dll, kernel32.dll, gdi32.dll, version.dll. VirtualAlloc, VirtualQuery, and GetKeyboardType     show memory allocation and interaction with the system.

   High number of imports that (375) show that it interacts with many windows API

   Flagged with multiple signatues

   Points to being packed and obfuscated

   ### X64dbg:

   See LdrInitializeThunk and ntdll.dll used by malware for process injection or thread manipulation.

   References to ZwQueryInformationThread, used for threat execution manipulation or analysis environment.

   Conditional jumps found in disassembly view, indicatives of unpacking or decrypting payloads on the fly.

   Mscoree.dll, ntdll.dll and kernelbase.dll are loaded, interacting deep into the system and components.

   ### Dependency Walker:

   Unresolved imports, and delay-load dependencies not being found. Often seen in packed or obfuscated malware.

   Actual calls appear to be resolved at runtime

   Circular dependency detected are indicators of malware using DLL hihacking

   Appears to be a packed trojan with process injection. It is packed as seen in dependenc walker.
