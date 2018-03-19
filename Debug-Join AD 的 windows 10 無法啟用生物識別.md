# AD-Note
* windows 10 生物識別無法套用在樹系功能等級2012以下的網域中，請直接修改註冊碼
```[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\System]
"AllowDomainPINLogon"=dword:00000001```
