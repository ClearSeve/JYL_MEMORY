# SSH

右键win图标，管理员方式打开powershell

Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'  
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

登录：ssh   uername@ip  

下载：scp  uername@ip:/root/1.txt   D:\1.txt

上传：scp D:\1.txt  username@ip:/root/1.txt
