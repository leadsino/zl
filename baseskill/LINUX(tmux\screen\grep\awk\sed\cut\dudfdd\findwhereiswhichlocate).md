# LINUX

### 1、tumx

tmux：创建一个session;

水平创建一个pane:先按下control键和b键，松开后，再按下%键；

垂直创建一个pane:control键和b键，松开后，再按下"键；

切换pane:按下control键和b键，松开后，再按下箭头按键；

全屏幕一个pane:C - b z；需要恢复的话再操作一次；

退出一个pane:exit；

创建一个新的windows:按下control键和b键，松开后，再按下c键
C - b <数字>来选择不同的window；

C - b d命令可以退出一个session。虽然退出了某个session，但tmux仍然保持该session在后台运行；

查看session：tmux ls
返回结果为：0: 1 windows (created Mon Apr 3 10:26:09 2017) [88x21]
重新进入session:tmux attach -t 0

### 2、screen

screen命令创建一个会话;

创建一个新的pane: ctrl+a 后再按c;

切换pane:ctrl+a 后再按n(向前切换)；ctrl+a 后再按p(向后切换);

退出pane:exit;

退出session,但仍然在后台运行：ctrl+a 后再按d;

screen -ls 查看当前的会话；
返回结果为   ：2399.pts-1.sullivan	(02/28/2019 11:34:11 AM)	(Detached);
进入session:   screen -r  2399.pts-1.sullivan (注意状态Attached和Detached区分是否是在screen中)

### 3、grep

grep  root /etc/passwd

将/etc/passwd，有出现 root 的行取出来；

grep -n root /etc/passwd

同时显示这些行在/etc/passwd的行号；

grep -v root /etc/passwd

将没有出现 root 的行取出来；

你可以在 ~/.bashrc 内加上这行：『alias grep='grep --color=auto'』再以『 source ~/.bashrc 』来立即生效即可,这样每次运行 grep 他都会自动帮你加上颜色显示；

grep  -r -l 'energywise' * 

-r文件夹递归查询,-l参数是返回文件名；

egrep '3+' testfile

grep -E '3+' testfile

grep '3\+' testfile 

三句话输出的结果是一样，正则表达式结果。

### 4、awk

grep 更适合单纯的查找或匹配文本

sed  更适合编辑匹配到的文本

awk  更适合格式化文本，对文本进行较复杂格式处理

awk [options] 'program' file1 , file2 , ```基本语法

awk [options] 'Pattern{Action}' file  对于program来说，又可以细分成pattern和action

awk '{print $5}' 1.txt

awk '{print $0}' 1.txt

awk '{print $NF}' 1.txt

那么NF的值就是7，$NF的值就是$7,而$7表示当前行的第7个字段；

awk '{print \$1,$2}' 1.txt

awk '{print "udev:",\$1,"blocks:",​$2,"test"}' 1.txt

 awk 'BEGIN{print "aaa","bbb"}{print \$1,​$2}' 1.txt 

BEGIN 表示在ACTION之前的操作；

awk '{print \$1,​$2} END{print "aaa","bbb"}' 1.txt

awk -F# '{print $2}' 1.txt

-F参数表示字段以#作为分割符号；

awk -F: '/root/{print $1}' /etc/passwd

正则表达式，先匹配符合的行，然后进行分割输出字符；

### 5、sed

-e<script>或--expression=<script> 以选项中指定的script来处理输入的文本文件。

 -f<script文件>或--file=<script文件> 以选项中指定的script文件来处理输入的文本文件。

a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～

c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！

d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；

i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；

p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～

s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！ 

sed -e 4a\newLine testfile 

 Sed  '4a  newline' testfile

两种写法，可以忽略-e参数，但是需要用单引号；

sed '3,$d'  testfile（以下略去文件名）

 sed '2,5d'

 sed '2a drink tea'

第二行后(亦即是加在第三行)加上『drink tea?』

sed '2,5c No 2-5 number'

 sed -n '5,7p'

 sed '/root/p' 

先查找

 sed  '/root/d'

```
sed 's/要被取代的字串/新的字串/g'
```

sed 's/^.*addr://g'

正则表达替换文件

### 6、cut

cut [option] files

### 7、du df dd

df -h 

命令用于显示目前在Linux系统上的文件系统的磁盘使用情况统计，-h参数用于以K，M，G显示大小；

du -h

显示当前目录下的所有文件目录大小；

du -ah

显示目录下所有文件目录与文件的大小；

du -sh * | sort -nr | head

显示文件目录中所有目录的大小并从大到小排序；

dd

dd可从标准输入或文件中读取数据，根据指定的格式来转换数据，再输出到文件、设备或标准输出;

```
dd if=testfile_2 of=testfile_1 conv=ucase 
```

if=文件名：输入文件名，缺省为标准输入。即指定源文件;of=文件名：输出文件名，缺省为标准输出。即指定目的文件;conv为转发，此处为转化为大写；

### 8、find whereis which locate

find 

从硬盘开始查找文件；

```
find . -name "*.c"
```

目前目录及其子目录下所有延伸档名是 c 的文件列出来

```
find . -type f |grep 'rule'
```

将目前目录其其下子目录中所有一般文件列出

```
find . -ctime -20
```

将目前目录及其子目录下所有最近 20 天内更新过的文件列出

find . -type f -perm 644

查找前目录中文件属主具有读、写权限查找文件；

Which

which指令会在环境变量$PATH设置的目录里查找符合条件的文件。

```
which bash
```

whereis

该指令只能用于查找二进制文件、源代码文件和man手册页，一般文件的定位需使用locate命令.

```
whereis bash 
```

locate

用于查找符合条件的文档，他会去保存文档和目录名称的数据库内，查找合乎范本样式条件的文档或目录.

```
locate passwd
```

```
locate -u 
```

一般是系统自己维护，手工升级数据库的方法;

