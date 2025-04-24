# 📁 Folder Organiser (PowerShell)

A simple PowerShell script that organises a folder by file type. Handy for quickly tidying up messy download folders or categorising documents.

---

## 🚀 Features

- Recursively scans a folder  
- Detects each file's extension  
- Creates folders for each file type  
- Moves files into folders like `pdf`, `txt`, `jpg`, etc.  
- Handles files with no extensions too  

---

## ⚙️ Usage

```powershell
.\Folder_Organiser.ps1 -Dir "C:\Path\To\Your\TargetFolder"
Replace C:\Path\To\Your\TargetFolder with the folder you want to organise.

# 🧪 Example
Before:
arduino
Copy
Edit
Downloads\
├── report.pdf
├── image.jpg
├── notes.txt
After running:
arduino
Copy
Edit
Downloads\
├── pdf\report.pdf
├── jpg\image.jpg
├── txt\notes.txt

#### 💻 Script
powershell
Copy
Edit
<#
Name       : Folder Organiser
Description: Organises a directory into a new location based on their file types.
#>

param (
    [Parameter(Mandatory = $true)]
    [string]$Dir
)

Write-Output "Organising files in: $Dir"

# Test if the directory exists
if (-not (Test-Path -Path $Dir)) {
    Write-Output "Directory does not exist: $Dir"
    exit
}

# Get only files (no folders)
$files = Get-ChildItem -Path $Dir -File

Write-Output "Found $($files.Count) files in $Dir."

# If no files found, exit
if ($files.Count -eq 0) {
    Write-Output "No files to organize."
    exit
}

foreach ($file in $files) {
    Write-Output "Processing file: $($file.Name)"

    # Use "NoExtension" if the file has no extension
    $fileType = if ($file.Extension) { $file.Extension.TrimStart('.') } else { "NoExtension" }

    Write-Output "File type: $fileType"

    # Destination path based on file type
    $destinationFolder = Join-Path -Path $Dir -ChildPath $fileType

    # Create the destination folder if it doesn't exist
    if (-not (Test-Path -Path $destinationFolder)) {
        Write-Output "Creating directory: $destinationFolder"
        New-Item -Path $destinationFolder -ItemType Directory | Out-Null
    }

    # Move the file to the folder
    Write-Output "Moving file: $($file.Name) to $destinationFolder"
    Move-Item -Path $file.FullName -Destination $destinationFolder
}

Write-Output "Folder organization complete!"

