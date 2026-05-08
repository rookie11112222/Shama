**==查看某个文件的大小==**
- ls -lh [文件名]


**==更改hostname==**
1. **su -(取得root权限)**
2. **vi /etc/hostname (把原来的hostname删掉后，填写新的hostname)**
3. **重启设备**


**==后台运行==**
- ./sh文件 &


**==查找功能==**
1. **vi编辑器打开后输入/**
2. **/后面输入想要查找的内容，例如/hostname，这样会直接跳到hostname单词上**
3. **往下查找按n**


**==ubuntu docker占用磁盘空间大==**
➣ df -h发现/var/lib/docker/overlay2/目录占用磁盘空间大
1. **cd /var/lib/docker/containers**
2. **ll -h**
3. **会发现好几个很长串文件名的文件**
4. **cd进去**
5. **>\*-json.log**
6. **重复4-5项，直到遍历所有文件**


**==ubuntu双网卡bonding作业==**
1. **编辑网卡配置**
```
vi /etc/network/interfaces
```
2. **按照如下配置**
```
auto bond0
iface bond0 inet static
address 10.117.183.229
netmask 255.255.255.0
gateway 10.117.183.1
dns-nameservers 10.117.4.30
bond-slaves none
bond-mode active-backup
bond-miimon 100

auto 主网卡名
iface 主网卡名 inet manual
bond-master bond0
bond-primary 主网卡名 副网卡名

auto 副网卡名
iface 副网卡名 inet manual
bond-master bond0
bond-primary 主网卡名 副网卡名
```
3. **编辑好后，重启网卡**
```
/etc/init.d/networking restart
```
4. **操作完成后，要把两个网卡的MAC地址发给网络担当者**


**==ubun系统忘记开机密码==**
1. **开机按键盘上的 Esc调出 GRUB Mode**
2. **选择recovery mode，也就是恢复模式**
3. **选择Drop to root shell prompt ,也就是获取root权限**
4. **在下面的# 后面敲入 cat /etc/shadow 查看用户名（如果连用户名都忘记的话）**
5. **passwd "用户名" 之后再敲两次密码就可以了。**
6. **重启**


**==列出所有PCI连接的设备信息==**
- lspci


**==列出Interface数据发送接收情况==**
- netstat -in


**==列出路由表==**
- netstat -rn


**==确认网络状态==**
- systemctl status network


**==确认/重启multipath服务==**
- systemctl reatart multipathd
- systemctl status multipathd


**==更改文件属性==**
➣ chgrp: 更改文件属组
- chgrp [-R] 属组名 文件名
- -R: 递归
➣ chown: 更改文件所有者，也可以更改文件所属组
- chown [-R] 所有者 文件名
- chown [-R] 所有者:属组名 文件名
➣ chmod: 更改文件9个属性
- chmod 777 文件名
- chmod u=rwx, g=rwx, o=rwx 文件名
- chmod u/g/o  +/-/=  r/w/x  文件名
- 例: chmod u+x 文件名(服务该文件名的所有者执行权限)


**==文件与目录管理==**
➣ ls(列出目录)
- -a: 全部文件，隐藏文件也包含
- -d: 仅列出目录本身，不列出目录内的文件数据
- -l: 长数据串列出，包含文件的属性与权限等
- ls -al 等同 ll
➣ pwd(显示目前所在的目录)
- -P: 显示出确实的路径，而非使用链接(link)路径
➣ mkdir(创建新目录)
- -m: 配置文件的权限
- -p: 直接将所需要的目录(包含上一级目录)递归创建起来
➣ rmdir(删除空目录)
- -p: 从该目录起，一次删除多级空目录


**==用户账号的管理==**
➣ useradd: 添加新的用户账号
- -c: 指定一段注释性的描述
- -d: 指定用户主目录
- -s: 指定用户的登录shell
- useradd -c test -d /home/test -m test
➣ userdel: 删除账号
- -r: 用户的主目录也一起删除
- userdel -r test
➣ usermod: 修改账号
- 参数和useradd一样


**==vi/vim编辑器使用==**
➣ 命令模式
- i: 切换到输入模式
- x: 删除当前光标所在处的字符
- a: 输入模式，在光标下一个位置输入文本
- o: 输入模式，在光标下一行输入文本
- dd: 剪切当前行
- yy: 复制当前行
- p(小写): 粘贴内容到光标下方
- P(大写): 粘贴内容到光标上方
➣ 输入模式
- BackSpace: 删除光标前一个字符
- Delete: 删除光标后一个字符
- Home: 光标移动到行首
- End: 光标移动到行尾
- Insert: 输入/替换模式切换用
➣ 一般模式
- G: 光标移动到最后一行
- gg: 光标移动到第一行
- /word: 向光标下搜索word字符串
- ?word: 向光标上搜索word字符串
- n: 重复前一个搜寻动作
- N: 反向重复前一个搜寻动作
- :%s/word1/word2/g  --  从第一行到最后一行寻找word1字符串，并替换为word2字符串
- :%s/word1/word2/gc  --  与上个命令基本相同，增加了提问的功能


**==检验某个Port通不通==**
- nc -zvw 1 [IP] [端口号]
```
nc -zvw 1 192.168.1.100 23
```


**==查看内存使用情况==**
```
free -h
```
- 不看free，要看available
- buffer/cache会分走一部分


**==java变量配置方法==**
1. **进入配置页**
```
vi /etc/profile
```
2. **最下面输入以下内容**
- export JAVA_HOME=/ENGN/cgssadm/SGP/SmartConnect/jdk-15.0.2(这个是java的安装路径)
- export PATH=\$JAVA_HOME/bin:$PATH
- export CLASSPATH=.:\$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
3. **保存后退出，重新登录**
4. **检验是否正常配置成功**
```
java -version
``` 
- jstack等命令验证


**==查看某个文件或者文件夹的大小==**
- du -h 1.txt


**==ksmtuned进程重启==**
1. **确认ksmtuned的PID号**
```
ps -ef | grep ksmtuned
```
2. **kill掉该进程**
```
kill -9 PID号
```
3. **启动ksmtuned的服务**
```
systemctl restart ksmtuned
```


**==NTP同步状态信息==**
- timedatectl status
- ntpq -p (使用ntpq同步时)
- ntpstat (使用ntpq同步时)
- chronyc sources (使用chronyc同步时)


**==查看某个端口运行的进程==**
- netstat -tunlp | grep 端口号


**==清空某个文件内的内容==**
- cat /dev/null > 文件名


**==iscsiadm常用命令==**
➣ 发现iscsi目标
- iscsiadm -m discovery -t st -p [IP地址]
➣ 登录到iscsi目标
- iscsiadm -m node -T [目标名] -p [IP地址] -l
- iscsiadm -m node --login
➣ 注销已登录的iscsi目标
- iscsiadm -m node -T [目标名] -p [IP地址] -u
- iscsiadm -m node --logout
➣ 显示当前登录的iscsi目标的会话
- iscsiadm -m session


**==查看linux服务器的相关信息==**
➣ CPU信息
- cat /proc/cpuinfo  
- lscpu
- nproc --all  # 查看核心数
➣ 内存信息
- free -h 
- cat /proc/meminfo
- dmidecode -t memory
➣ 磁盘信息
- df -h  # 查看已挂载磁盘使用情况 
- lsblk  # 查看块设备信息
- fdisk -l  # 查看磁盘分区(需要root权限)
- hdparm -i /dev/sda  # 查看磁盘详细信息 
➣ 网络接口
- ip a    
- ifconfig -a  
- ethtool [interface]  # 查看网卡详细信息
➣ 系统信息
- uname -a  # 查看内核和系统信息
- lsb_release -a  # 查看发行版信息(可能需要在某些系统上安装lsb-release)   
- cat /etc/os-release  # 查看操作系统信息   
- hostnamectl  # 系统主机名和相关信息
➣ PCI设备
- lspci
- lspci -v  # 更详细信息
- lspci -tv  # 树状显示
➣ USB设备
- lsusb
- lsusb -v  # 更详细信息
➣ BIOS信息
- dmidecode -t bios
➣ 型号和制造商
- dmidecode -t system
- cat /sys/class/dmi/id/product_name
➣ 综合工具
- lshw  # 需要root权限获取完整信息
- inxi -F  # 综合信息(可能需要安装inxi)
- hwinfo --short  # 硬件信息(可能需要安装hwinfo)