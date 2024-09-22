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

      
