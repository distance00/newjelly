## win7系统开启ipv6的方法 （其它window系统类似）

### 方法 1
保存下面命令为bat文件，以管理员身份双击执行
```
@echo off

net start "ip helper"
netsh int ipv6 reset

netsh int teredo set state default
netsh int 6to4 set state default
netsh int isatap set state default
netsh int teredo set state server=teredo.remlab.net
netsh int ipv6 set teredo enterpriseclient
netsh int ter set state enterpriseclient
route DELETE ::/0
netsh int ipv6 add route ::/0 "Teredo Tunneling Pseudo-Interface"
netsh int ipv6 set prefix 2002::/16 30 1
netsh int ipv6 set prefix 2001::/32 5 1
Reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Dnscache\Parameters /v AddrConfigControl /t REG_DWORD /d 0 /f

netsh int teredo set state default
netsh int 6to4 set state default
netsh int isatap set state default
netsh int teredo set state server=teredo.remlab.net
netsh int ipv6 set teredo enterpriseclient
netsh int ter set state enterpriseclient
route DELETE ::/0
netsh int ipv6 add route ::/0 "Teredo Tunneling Pseudo-Interface"
netsh int ipv6 set prefix 2002::/16 30 1
netsh int ipv6 set prefix 2001::/32 5 1
Reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\Dnscache\Parameters /v AddrConfigControl /t REG_DWORD /d 0 /f

ipconfig /all
ipconfig /flushdns
netsh int ipv6 show teredo
netsh int ipv6 show route
netsh int ipv6 show int
netsh int ipv6 show prefix
netsh int ipv6 show address
route print
cmd
```

### 方法 2 （手动，更直观）

1 计算机 “管理”----“服务和应用程序”-------“服务”-------启用IP helper

2 打开组策略 gpedit.msc
‘’计算机配置‘’------管理模版----“网络”-----‘’TCPIP设置‘’------‘’IPV6转换技术-----‘’ISATAP状态‘’设置为“已启用‘’
‘’TEREDO服务器名称‘’设置‘’已启用 ‘’，3个服务器中选一个设置：teredo.remlab.net ------teredo-debian.remlab.net----teredo.trex.fi
‘’TEREDO状态‘’设置为“已启用‘’ 选择企业客户端

