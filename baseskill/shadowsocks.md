# shadowsocks

## client:

![shadowsocks_client](https://github.com/leadsino/zl/blob/master/image/shadowsocks_client.gif)

https://free-ss.site/ 在线免费的ss服务器网站

## server:

在Ubuntu下安装SS很简单，只需要依次执行下面3条命令：

```
apt-get update
apt-get install python-pip
pip install shadowsocks
```

写一个配置文件保存为 etc/shadowsocks.json，文件内容如下：

```
{
    "server":"你服务器外网IP地址",
    "server_port":自定义端口号,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```
注意事项：
1、报错：AttributeError: /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1: undefined symbol: EVP_CIPHER_CTX_cleanup
处理方式：修改/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py文件中的EVP_CIPHER_CTX_cleanup修改为EVP_CIPHER_CTX_reset,这是由于在openssl 1.1.0中废弃了 EVP_CIPHER_CTX_cleanup() 函数而引入了 EVE_CIPHER_CTX_reset() 函数所导致的;
2、shadowsocks.json配置文件中的外网服务器ip，可能需要填写0.0.0.0;

配置文件编辑完成后，接下来就可以部署运行了:

```
ssserver -c /etc/shadowsocks.json -d start
```

当然如果你想添加多个账号的话，可以编辑shadowsocks.json配置文件

```
{
    "server":"your_server_ip",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
        "8989":"password0",
        "9001":"password1",
        "9002":"password2",
        "9003":"password3",
        "9004":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

编辑好了之后输入`ssserver -d restart` 重启SS。

