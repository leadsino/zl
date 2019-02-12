# wireguard

## 一、搭建google云主机

![](https://github.com/leadsino/zl/blob/master/image/google%20%E4%B8%BB%E6%9C%BA%E9%85%8D%E7%BD%AE.png)

![](https://github.com/leadsino/zl/blob/master/image/google%20vps%E7%BD%91%E7%BB%9C%E9%98%B2%E7%81%AB%E5%A2%99.png)

服务器端配置好后，需要配置网络防火墙wireguard的端口映射

## 二、服务器端配置

 1、使用SSH客户端（xshell），以root登录后执行下面代码

```
$ yum install -y wget
$ wget https://raw.githubusercontent.com/yobabyshark/wireguard/master/wireguard_install.sh && chmod +x wireguard_install.sh && ./wireguard_install.sh
```

2、在弹出的对话框输入1，更新内核，安装完成后按照提示重启。

3、VPS重启成功后，执行下面代码，根据提示输入2进行wireguard的安装

```
$ ./wireguard_install.sh
```

4、wireguard安装完成后，该服务就自动启动了，可以通过命令 `wg` 检查启动是否成功，成功的话会输出如下内容：

```
interface: wg0
  public key: xxxxxxxxxx
  private key: (hidden)
  listening port: xxxxx
peer: xxxxxxxxxxxxxxxxx
allowed ips: 10.0.0.2/xx
```

5、把client.conf的内容复制到记事本，另存xxx.conf文件。这是客户端的各种参数，Android、iOS、mac、linux、PC客户端也是使用这些参数。

```
$ vi /etc/wireguard/client.conf
```

## 三、客户端配置

### mac

1、安装fireguard-tools

`brew install wireguard-tools`

2、建立文件夹，并把服务器端/etc/wireguard/client.conf信息复制到wg.conf中

```sudo mkdir /etc/wireguard
sudo mkdir /etc/wireguard
sudo chmod 777 /etc/wireguard/
cd /etc/wireguard/
touch wg.conf
```

3、开启和关闭wireguard

```wg-quick up wg
wg-quick up wg
wg-quick down wg
```

## windows

1、登陆官网下载，并安装

TunSafe-TAP-9.21.2.exe

TunSafe-1.4-x64.zip

2、解压后在tunsafe文件目录下的conifg文件中放置wg.conf

3、运行tunsafe，选择配置文件为wg连接即可

