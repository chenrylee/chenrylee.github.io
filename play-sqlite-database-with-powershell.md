# Play SQLite Database with PowerShell

## 0. Prerequisites
- Windows 10 or Windows Server 2016 and above
- PowerShell 5.0 and above
If you are running a lower version, please refer to [Microsoft Docs](https://docs.microsoft.com/en-us/powershell/gallery/installing-psget)

## 1. Install The Module
### 1.1 Install Nuget
```powershell
Install-PackageProvider Nuget – Force
```
### 1.2 Install PowerShellGet
```powershell
Install-Module – Name PowerShellGet – Force
```
### 1.3 Set the Repository Trusted
```powershell
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
```
### 1.4 Install Module *PSLite*
```powershell
Install-Module PSSQLite -Force
```
## 2 How Do I Know It Works?
```powershell
Get-Command -Module PSSQLite

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Invoke-SQLiteBulkCopy                              1.0.3      PSSQLite
Function        Invoke-SqliteQuery                                 1.0.3      PSSQLite
Function        New-SQLiteConnection                               1.0.3      PSSQLite
Function        Out-DataTable                                      1.0.3      PSSQLite
```
## 3. Let's Begin
### 3.1 Create a database and table
```powershell
$database = "D:\Scripts\DB\test.sqlite"
$query = "
        CREATED TABLE Names (
        fullname VARCHAR(20) PRIMARY KEY,
        surname TEXT,
        givenname TEXT,
        birthdate DATETIME
        )"
Invoke-SqliteQuery -Query $query -DataSource $database
Invoke-SqliteQuery -DataSource $database -Query "PRAGMA table_info(names)" | Format-Table
cid name      type        notnull dflt_value pk
--- ----      ----        ------- ---------- --
  0 fullname  varchar(20)       0             1
  1 surname   text              0             0
  2 givenname text              0             0
  3 birthdate datetime          0             0
```
### 3.2 Insert Data and Display
```powershell
$query = "INSERT INTO Names (fullname,surname,givenname,birthday)
                      VALUES (@full, 'Cookie', 'Monster', @bd)"
Invoke-SqliteQuery -DataSource $database -Query $query -SqlParameters @{full = "Cookie Monster"; BD = (Get-Date).AddYears(-11)}
Invoke-SqliteQuery -DataSource $database -Query 'select * from names'
fullname       surname givenname birthdate
--------       ------- --------- ---------
Cookie Monster Cookie  Monster   6/4/2008 10:00:25 AM
```
Look, `fullname` and `birthdate` are passed from the parameters.
