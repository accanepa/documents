# PowerShell 101

## Windows PowerShell
```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```
> <img src="https://i0.wp.com/windowsloop.com/wp-content/uploads/2021/12/open-windows-terminal-as-admin-201221.jpg?resize=1024%2C573&ssl=1" alt="drawing" width="800"/>

## Windows PowerShell ISE
```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell_ise.exe
```
> <img src="https://user.oc-static.com/upload/2022/04/26/16509977637453_image107.png" alt="drawing" width="800"/>

## Clear | Write | Variables
#### `Write-Host`
```powershell
Write-Host "Hello World"
```
#### Logic
```powershell
Clear-Host

Write-Host "Hello $env:username `n"

# store username in variable 
$username = $env:username
Write-Host "`$username = $username `n"

# store string in variable
$myvar = "This Is Easy"
Write-Host "$username $myvar`n"

# last trick
Write-Host "$($env:username)"
Write-Host "$(whoami)"
```

### `Challange` Print The Path To Your Desktop
```
C:\Users\Administrator\Desktop
```

## Test | Create | Remove

#### `Test-Path`
```powershell
Test-Path "C:\Temp" -PathType Container
```

#### `New-Item`
```powershell
New-Item "C:\Temp" -ItemType Directory
```
```powershell
New-Item "C:\Temp" -ItemType Directory -Force
```
```powershell
New-Item "C:\Temp" -ItemType Directory -Force | Out-Null
```

#### `Remove-Item`
```powershell
Remove-Item "C:\Temp"
```

#### `if` and `else` Statements
```powershell
if(<<is this $True or $False>>){
    <<when $True do this>>
}
# optional
else{
    <<when $False do this>>
}
```

#### Logic
```powershell
Clear-Host

# if Temp doesn't exist create it
if(Test-Path "C:\Temp" -PathType Container){
    Write-Host "[Directory] C:\Temp Exists"
}
else{
    New-Item "C:\Temp" -ItemType Directory -Force | Out-Null
    Write-Host "[Directory] C:\Temp Created"
}

# create bad.txt
New-Item "C:\Temp\bad.txt" -ItemType File -Force | Out-Null
Write-Host "[Created] C:\Temp\bad.txt"

# if bad.txt exists remove it
if(Test-Path "C:\Temp\bad.txt" -PathType Leaf){
    Remove-Item "C:\Temp\bad.txt" -Force
    Write-Host "[Removed] C:\Temp\bad.txt"
}
```

### `Challange` Create C:\Temp Directory & A Blank File Named After Your Username
```
C:\Temp\Administrator.txt
```

## Website Requests

#### `Invoke-Webrequest`
```powershell
Invoke-WebRequest "https://api.ipify.org"
```

#### `Select-Object` & `|`
```powershell
$properties = Invoke-WebRequest "https://api.ipify.org" | Select-Object *

$properties

$properties.Content
```
```powershell
Invoke-WebRequest "https://api.ipify.org" | Select-Object Content
```
```powershell
(Invoke-WebRequest "https://api.ipify.org").Content
```

#### Using Content
```powershell
$request = Invoke-WebRequest "https://api.ipify.org" -UseBasicParsing
$ip = $request.Content
Clear-Host
Write-Host "StatusCode = $($request.StatusCode)"
Write-Host "IP = $ip" 
```

### `Challange` Print the Content if the StatusCode is 200
```
xxx.xxx.xxx.xxx
```

#### Downloading Files
```powershell
Invoke-WebRequest "https://api.ipify.org" -OutFile "C:\Users\$env:username\Desktop\ip.txt"
```

#### `Out-File`
```powershell
(Invoke-WebRequest "https://api.ipify.org").Content | Out-File "C:\Users\$env:username\Desktop\ip.txt"
```
```powershell
(Invoke-WebRequest "https://api.ipify.org").Content > "C:\Users\$env:username\Desktop\ip.txt"
```

### `Challange` Using the Content of the Request Create a File Named the IP
```
C:\Users\Administrator\Desktop\xxx.xxx.xxx.xxx.txt
```

## Starting Processes
#### `Start-Process`
```powershell
Start-Process notepad.exe -Wait
```

### `Challange` Make This Script Work
```powershell
$filepath = "C:\Users\$env:username\Desktop\ip.txt"

Invoke-WebRequest "https://api.ipify.org" -OutFile $filepath
Start-Process notepad.exe -ArgumentList $filepath

Remove-Item $filepath
```

## Reading files
### `Get-Content`
```powershell
$filepath = "C:\Users\$env:username\Desktop\ip.txt"
Invoke-WebRequest "https://api.ipify.org" -OutFile $filepath
Get-Content $filepath
```

### `Challange` Using Get-Content Store the Result as a Variable and Print to Screen
```powershell
$filepath = "C:\Users\$env:username\Desktop\ip.txt"
Invoke-WebRequest "https://api.ipify.org" -OutFile $filepath
Get-Content $filepath
```

## Foreach Loops
```powershell
foreach($_ in (1..10)){
    Write-Host $_
}
```