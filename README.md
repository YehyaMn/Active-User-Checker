# Active-User-Checker
A PowerShell script to quickly check which users are currently logged in on multiple Active Directory computers using `qwinsta`. The script supports wildcards for computer names and automatically skips unreachable PCs

# Features

Query multiple computers at once using AD.
Show only active user sessions.
Skip unreachable computers to save time.
Hide errors for cleaner output.
Works with wildcards like PC-*.

# Requirements

Windows PowerShell
Active Directory module (RSAT-AD-PowerShell installed)
Permission to query remote computers ( Domain-Admin)

# Usage

Clone the repository or copy the script.

Open PowerShell and run:
```
$pattern = Read-Host "Enter computer name or wildcard (use * for wildcard)"

# Get all matching AD computers
$computers = Get-ADComputer -Filter "Name -like '$pattern'" | Select-Object -ExpandProperty Name

foreach ($c in $computers) {
    Write-Host "`n=== $c ===" -ForegroundColor Cyan

    if (Test-Connection -ComputerName $c -Count 1 -Quiet) {
        qwinsta /server:$c 2>$null | Select-String "Active"
    } else {
        Write-Host "Skipped: $c is unreachable" -ForegroundColor Yellow
    }
}


Enter a computer name or wildcard pattern, e.g. PC-*.

The script will list active users on each reachable PC.

Example Output
=== PC-001 ===
console                   Firstname.Lastname                2  Active

=== PC-002 ===
console                   Firstname.Lastname                   1  Active

=== PC-003 ===
Skipped: PC-003 is unreachable
```
# Contributing

Feel free to fork this repository and submit pull requests to improve functionality, add features, or support more filters.
