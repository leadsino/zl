### tail

tail -f /var/log/messages #动态查看日志

tail -20 REQUEST-910-IP-REPUTATION.conf #查看文档后20行

### head

head -20 REQUEST-910-IP-REPUTATION.conf  #查看文档前20行

### lsof(重要)

1、可以查看网络连接；2、可以查看文件占用；3、可以查看进程文件关系

lsof -i   #查看端口状态

lsof -i :22   #端口过滤连接

lsof /usr/bin/mysqld_safe   #文件被进程占用；

lsof -p 2200    #基于PID查看占用的文件情况；

kill  -9   \`lsof -t -u daniel\`  #查找制定用户进程杀死

### dig

dig www.test.com   #DNS查询IP

dig +trace www.test.com #详细的递归记录

dig @8.8.8.8 www.test.com  #制定DNS服务器查询

### strings

查看二进制文件中的内容

strings head |grep 'License GPLv'   #查询二进制文件中制定的内容

### mount

linux系统中，硬盘需要挂载到制定文件系统目录下才能有接口访问。

mount  /dev/sdb1 /app/sdb1 #挂载操作

以下是卸载的操作

lsof /app/sdb1 #查看挂载文件目录下进程占用情况，如果有进程占用是无法卸载的

fuser -km /app/sdb1  #终止所有的目录占用进程

umount  /dev/sdb1 #卸载磁盘

### tar

-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

-z：有gzip属性的
-j：有bz2属性的
-Z：有compress属性的
-v：显示所有过程
-O：将文件解开到标准输出

tar -cvf jpg.tar *.jpg //将目录里所有jpg文件打包成jpg.tar 

tar -xvf file.tar //解压 tar包

tar -tf all.tar  #查看tar包中的文件

tar -czf jpg.tar.gz *.jpg   //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩

tar -xzvf file.tar.gz //解压tar.gz

tar -cjf jpg.tar.bz2 *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩

tar -xjvf file.tar.bz2   //解压 tar.bz2

rar a jpg.rar *.jpg //rar格式的压缩，需要先下载rar for linux

unrar e file.rar //解压rar

zip jpg.zip *.jpg //zip格式的压缩，需要先下载zip for linux

unzip file.zip //解压zip







