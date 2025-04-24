# 📁 Folder Organiser (PowerShell)

A simple PowerShell script that organises a folder by file type.

The script will:

Scan the target directory for files

Create folders for each file extension within that directory

Move files into the correct folders

## 🔧 Usage

```powershell
.\Folder_Organiser.ps1 -Dir "C:\Path\To\Your\TargetFolder"


## 📜 PowerShell Script

```powershell
<#
Name       : Folder Organiser
Description: Organises a directory into a new location based on their file types.
#>

param (
    [Parameter(Mandatory = $true)]
    [string]$Dir
)

Write-Output "Organising files in: $Dir"

if (-not (Test-Path -Path $Dir)) {
    Write-Output "Directory does not exist: $Dir"
    exit
}

$files = Get-ChildItem -Path $Dir -File

Write-Output "Found $($files.Count) files in $Dir."

if ($files.Count -eq 0) {
    Write-Output "No files to organize."
    exit
}

foreach ($file in $files) {
    Write-Output "Processing file: $($file.Name)"
    $fileType = if ($file.Extension) { $file.Extension.TrimStart('.') } else { "NoExtension" }
    $destinationFolder = Join-Path -Path $Dir -ChildPath $fileType

    if (-not (Test-Path -Path $destinationFolder)) {
        Write-Output "Creating directory: $destinationFolder"
        New-Item -Path $destinationFolder -ItemType Directory | Out-Null
    }

    Write-Output "Moving file: $($file.Name) to $destinationFolder"
    Move-Item -Path $file.FullName -Destination $destinationFolder
}

Write-Output "Folder organization complete!"
