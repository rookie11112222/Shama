==**Manager服务器账号密码**==
- 10.72.205.21
- xappeoq / P@ssw0rd
- vms_adm01 / lgdco201804!
- sys_adm01 / lgdco201804!


**==Manager网页密码==**
- admin / 1Q2w3e4r5t
- 姓名: Administrator
- 联系方式: 02-1111-1111
★**密码找回**
- 提问: password reset question
- 回答: password reset answer


**==Manager DB连接==**
- 密码: 1234
- 需要用vms_adm01或者sys_adm01账号连接


**==Dpro网页账号密码==**
- root / LGDCOdspt2020!


**==Storage相关==**
➣ #1 / #3 / #5
- Dell存储管理软件
➣ #6
- 网页进去
- manage / P@ssw0rd


**==CCTV网页密码==**
- 海康/Axis: LGDco201809!
- LG: LGDco201809


==**移动CCTV WiFi账号密码**==
- LGD_DMS / LGD_DMS20191024


**==VMS服务启动顺序==**
1. **postgresql-x64-9.3**
2. **MongoDB Server**
3. **LG CNS Intelli-VMS Nginx**
4. **LG CNS Intelli-VMS PTZ Manager**
5. **LG CNS Intelli-VMS EVENT Manager**
6. **LG CNS Intelli-VMS Manager**
7. **LG CNS Intelli-VMS Manager Backend**
- 按照上述顺序启动，有依赖项
- 服务停止顺序和启动顺序相反


**==CCTV信息==**
➣ 海康
- 初始IP: 192.168.1.64
- 初始账号密码: admin / admin

➣ Axis
- 初始IP: 192.168.1.10
- 初始账号密码: root / root

➣ LG
- 初始ip: 192.168.0.16
- 初始账号密码: admin / admin or 123456


**==海康摄像头rtsp地址==**
- rtsp://admin:password@ip:554/streaming/channels/101
- admin : 摄像头账号
- password : 摄像头密码
- 如rtsp端口为默认，554可以省略
- streaming/channels可变更为streamer/channel互换
- 101为主码流，102/103为子码流


**==海康威视官网 / 大华官网==**
- 15840944875 / Wcyx1990!


**==CMS室 Radmin PC==**  
- ID: cosafetysupervisor
- PW: aqb@202601
★ **rdmin信息**
- COCMS / 1Q2w3e4r5t!


**==Dpro上的CCTV录像状态一直是streaming==** 
➣ 重启Dpro也一样
➣ /mnt/recorder目录下没有生成.mount文件
1. **运行如下命令**
```
touch /mnt/recorder/.mount
```


**==摄像头镜头焦距的选择==**
![[mmexport1691666248855.jpg]]
![[mmexport1691666252456.jpg]]
![[mmexport4a9cae5a86af67ec635bad857d6aa284_1691666255274.jpeg]]


**==摄像头磁盘相关==**
![[Screenshot_2023-08-10-19-18-37-49_e39d2c7de19156b0683cd93e8735f348.jpg]]
![[mmexport1691666473059.png]]


**==名词解释==**
CCTV: Closed-Circuit Television (闭路电视)
VMS: Video Management System
PSIM: Physical Security Information Management


**==VMS上的CCTV名字长度==**
- 最多100个字符(中英文，空格，特殊符号)
- 24寸显示器单一画面最多显示80个左右字符


**==韩华CCTV信息==**
- 初始IP: 192.168.1.100
 - rtsp://{IP地址}/profile2/media.smp


**==Dpro网卡状态确认==**
➣ cat /sys/class/net/eth0/carrier
- 结果为1表示网卡物理连接正常
- 结果为0表示网卡物理连接异常
➣ ethtool eth0
- 显示结果最下面的Link Detected为yes表示连接正常，结果为no表示连接异常


**==Dpro/Dpro2 SSD故障==**
➣ Dpro开机报错: error: failure reading sector 0xc78 from 'hd0'
1. **更换SSD**
2. **配置作业(更换Sub Board的时候做的更新MAC地址的操作)**
3. **确认各个板子的FW版本，统一一下**
4. **把该Dpro/Dpro2备份的db复制到主板/data/db目录下并改名为ONVIF.sqlite**
- 备份的db在VMS Manager上的路径:E:\ENGN\Intelli-VMS\Manager\files\distributor，从里面用uuid寻找对应db
- 上传的时候用ALZip
- mv命令移动及改名
5. **Manager网页确认Dpro的uuid后，在Dpro上更改**
- /userdata/onvif/bin/configuration/cluster.xml
 6. **重新联动Storage**
- 因为已经有volumn，所以要跳过mkfs命令，一旦运行mkfs命令，此前的数据都会丢失(mkfs为格式化volumn的命令)
- 一定要解绑，不然IP会不显示
- 要在dpro/dpro2上设置NTP和时区


**==VMS Viewer License删除query==**
➣ 查询最后login时间为空的Viewer
```
select * from tb_license  
where device_type = 'viewer' 
and last_access_time is null
```

➣ 查询最后login时间为12/30之前的Viewer
```
select * from tb_license
where device_type = 'viewer' and substring(to_char(last_access_time, 'YYYY-MM-DD'),1,10) < '2023-12-30'
```

➣ 删除最后login时间为空的viewer
```
delete from tb_license
where device_type = 'viewer
and last_access_time is null
```

➣ 删除最后login时间为12/30之前的viewer
```
delete from tb_license
where device_type = 'viewer' and substring(to_char(last_access_time, 'YYYY-MM-DD'),1,10) < '2023-12-30'
```


**==CCTV主要参数==**
➣ 图像传感器
- 分为CCD和CMOS，现在都用CMOS
- 尺寸越大，感光性能越好
- 1/1.8″=(1/1.8)16=8.89mm，这里的一英寸不是25.4mm，而是16mm

➣ 最低照度
- 摄像机的灵敏度，摄像机能够识别被摄物体的最低照度
- 一般分为星光级(0.01-0.001Lux，摄像机可以保持全彩)，超星光级(0.01-0.0005Lux，摄像机可以保持全彩)，黑光级(0.0005-0Lux，摄像机可以保持全彩)
- 0Lux with IR表示0Lux情况下，采用IR补光

➣ 焦距(f)
- 镜头和感光元件的距离，改变焦距，可以改变镜头的放大倍数
- 镜头的放大倍数=焦距/物距
  
➣光圈
- 安装在镜头后部，光圈越大，通过镜头的光量越大，图像清晰度越高
- 通常用F(光通量)表示，F后面的数字越小，光圈越大
- F=焦距(f)/通光孔径

➣ 白平衡
- 不管在任何光源下，都能将白色物体还原为白色的能力

➣ 信噪比
- 单位为db
- 数值越大，图像质量越好

➣ 3D数字降噪(3D DNR)
- 由于图像噪波的出现是随机的，因此每一帧图像出现的噪波是不相同的。3D数字降噪通过对比相邻的几帧图像，将不重叠的信息(即噪波)自动滤出，减少了噪点，使得图像更加细腻。

➣ AGC(Automatic Gain Control)
- 自动增益控制
- 将来自CCD的信号放大到可以使用水准的放大器，其放大量即增益。

➣ 背光补偿
- 把画面分成几个不同的区域，每个区域分别曝光。

➣ 宽动态
- 指图像在同一时间曝光两次：一次快，一次慢。再进行合成使得能够同时看清画面上亮与暗的物体。
- 宽动态技术和背光补偿技术都是为了克服在强背光环境条件下，看清目标而采取的措施，但背光补偿是以牺牲画面的对比度为代价的，从某种意义上说，宽动态技术是背光补偿技术的升级版。

➣ 数字变倍/光学变倍
- 数字变倍的倍数越大，画质就越差。因为数字变倍实际上是对采集到的图像进行局部放大，受限于采集图像的分辨率。
- 光学变倍的倍数不影响画质，光学变倍是通过相机镜头中的镜片组的移动变换不同的焦距来实现物体的放大缩小，本质上是望远镜的功能。

➣ 日夜转换模式：ICR
- ICR: 双滤光片切换器，是用于让滤光片白天切换到不感红外滤光片，晚上切换到感红外滤光片的红外摄像机配件。


**==CCTV主要硬件==**
➣ 镜头(透光能力)
- 决定了光线透过镜头玻片的衰减率

➣ 光圈(进光能力)
- 决定了相同时间内摄像头接收到的进光量

➣ 图像传感器(感光能力)
- 图像传感器越大，像素点越大，感光性能越强

➣ 补光灯(补光能力)
- 补光灯的类型和数量决定了摄像头的种类
- 分为红外(图像黑白)，白光(图像彩色，较刺眼)，暖光(图像彩色，较柔和)


**==scp命令(主板间传东西)==**
```
scp -P 22 /etc/multipath.conf root@10.72.207.31:/etc
```
- 在要传东西的主板运行
- 把本地/etc/multipath.conf文件传输到10.72.207.31的/etc目录，以root账号登录


**==CCTV移动(A Dpro to B Dpro)==**
➣ A故障，B为新的整机
1. **将A Dpro的db文件移过去**
- 路径及方法参考SSD故障
2. **把新Dpro的IP设为原来故障Dpro的IP**
3. **重新联动Storage**
- mkfs命令不能运行


**==Axis移动CCTV初始化方法==**
1. **断电**
2. **右侧两个键中按住上面的键，同时上电**
3. **放开后，过30s~1Min，插网线**
- 客服电话: 400-920-6676


**==Dpro kill 录像进程==**
```
killall -9 nr_streamer
```
 

**===DST/Dpro/Dpro2变更Sub Board密码**
```
mount -o remount,rw /
passwd
```


**==Dpro/Dpro2更换Sub Board==**
1. **确认要更换的子板(实际故障子板和标识的子板序号有可能不同)**
2. **更换子板**
3. **telnet连接更换后的子板**
 - root / qhdksWP114
4. **输入下面命令**
```
sed -i 's/1/0/g' /userdata/onvif/bin/configuration/pingchk.conf 
```
- 不输入此命令，Sub Board会一直重启，一ping通就马上telnet后输入命令
5. **变更Sub Board只读权限**
```
mount -o remount,rw /
```
6. **更改start_app配置**
```
vi /etc/init.d/start_app
```
- arptables -A INPUT -d 10.123.14.x -j DROP更改x部分(Sub#1 - 10.123.14.1以此类推)
- ./vms_dst_subsystem configuration/subsystemX.xml更改X部分(Sub#5  -  4，以此类推)
- ./watchdog -s X -b 9600 -T & 更改X部分(Sub#5  -  4，以此类推)
7. **查看并添加读/写/执行权限**
- chmod u+x 文件名给权限
- ls -l (正常的话是绿色的)
8. **确认Sub Board的MAC地址**
```
ifconfig
```
- eth1 - HWaddr
9. **更改static_arp.sh配置**
```
vi /etc/init.d/static_arp.sh
```
- telnet进每个子板，换成更换后的子板的Mac地址
- ssh进主板，换成更换后的子板的Mac地址
- 子板 - 10.123.14.1~8
- 主板 - 10.123.14.9
10. **查看并添加读/写/执行权限**
- 重复第7步
11. **更改更换后的Sub Board的内部/外部IP**
```
vi /etc/network/interfaces
```
- 更改内部和外部IP，具体参考其他子板
- IP设置后，邀请网络担当者解绑
12. **更改user.conf配置**
 ```
 vi /userdata/onvif/bin/configuration/user.conf
 ```
- 改成和其他子板一样
13. **重启一下dpro后上架**


**==Dpro/Dpro2 主板/子板固件升级==**
**1. 将固件软件放到相关目录**
- 主板:/root/update
- 子板:/Temp
- 用alzip软件ftp上传上去
- 主板: 22 / sftp     子板: 21 / ftp
**2. 解压文件**
- unzip 文件名
**3. 赋予文件权限**
- chmod u+x 文件名(.sh文件)
**4. 运行文件**
- ./文件(sh文件)
- 主板要运行2个软件，一个固件，一个存储相关的
- 子板运行一个即可，子板会自动重启2次左右
**5. 确认升级后的固件版本**
```
cat /data/log/sif_version
```
- Dpro/Dpro2都有2.7.2和2.7.4版本
- 2.6.0的只有Dpro有
- Dpro建议不要升2.7.4，Viewer上会提示==**stream账号登录失败==**，用VLC软件使用stream/1111时无法打开


**==DST/Dpro/Dpro2 FTP相关==**
- 主板: server/client
- 子板: server
- server: 允许别人往你这上传或下载东西
- client: 你可以去别人那上传过下载东西


**==应急管理局NVR==**
➣ CA
- 10.72.254.101 / 183.236.68.130
- 前面为LGD内网IP，后面为公网IP

➣ CO
- 10.72.254.102 / 183.236.68.131
- 前面为LGD内网IP，后面为公网IP

➣ CO/CA
- 10.72.254.100 / 183.236.68.132
- 前面为LGD内网IP，后面为公网IP

➣ NVR其他信息
- 子网掩码: 255.255.255.240
- 默认网关: 10.72.254.110
- 账号: admin
- 密码: 31B5B0rUbp5G!


==**curl命令(子板之间传文件)**==
```
curl -T /Temp/test.sh -u root:qhdksWP1 ftp://10.117.183.224/../../Temp/
```
- 本机的/Temp/test.sh文件传输到10.117.183.224的/Temp目录
- root:qhdksWP114是10.117.183.224的账号密码
- /../..是必须的


**==Dpro/Dpro2 Default IP==**
- Cluster: 192.168.0.169
- Sub1~Sub8: 192.168.0.161 ～168


**==Dpro/Dpro2 - Dell Storage==**
1. **确认eth0 / eth2的Mac地址**
```
ip link show
```
2. **eth0/eth2 配置修改**
```
cd /etc/sysconfig/network-scripts
vi ifcfg-eth0
```
- DEVICE=eth0
- UUID前面加#注释掉
- ONBOOT=yes
- HWADDR填写第一步确认到的Mac地址
- NAME="System eth0"
- IPADDR / NETMASK设置为和storage同网段的IP
- 按照上面的设置eth2
- 如果没有ifcfg-eth2
```
cp ifcfg-eth0 ifcfg-eth2
```
3. **基本设定**
```
vi /etc/iscsi/initiatorname.iscsi
```
- redhat后面更改为该Dpro/Dpro2的hostname
```
vi /etc/iscsi/iscsid.conf
```
- node.session.timeout.replacement_timeout更改为30
- vi编辑器里进入编辑模式后，输入/后，搜索replacement就可以
```
systemctl enable iscsi
systemctl enable iscsid
systemctl enable multipathd
systemctl disable smmonitor
systemctl disable smagent
```
4. **确认multipath.conf文件存在与否**
- 路径: /etc/multipath.conf
- 如果没有的话，从其他Dpro上复制一个过去
```
scp -P 22 /etc/multipath.conf root@192.168.0.169:/etc
```
5. **storage联动**
```
iscsiadm -m discovery -t st -p 192.168.50.230
```
- 192.168.50.230为storage IP，去其他联动到该storage的dpro上用history命令确认
- 正常查找到storage的话，会出现该storage的相关信息
```
iscsiadm -m node --login
```
- 正常联动成功的话，会提示success
```
fdisk -l
```
- 确认volumn正常连接与否，正常连接结果会显示Disk /dev/mapper/mpathb
```
mkfs.xfs -f /dev/mapper/mpathb
```
- 将连接到的volumn格式化为xfs格式
6. **Dpro/Dpro2 Storage自动连接设定**
```
mkdir /mnt/recorder
```
- /mnt目录下创造recorder文件夹
```
vi /recorder/mount-storage.sh
```
- mode选项中，把SAN前面的注释去掉
- 确定mountdev1="/dev/mapper/mpathb"和mountpos1="/mnt/recorder"是否正确
```
/recorder/mount-storage.sh
```
- 进行mount作业
```
df -h
```
- 确认/mnt/recorder是否正常mount上
```
touch /mnt/recorder/.mount
```
- 生成必须的文件
7. **测试数据读写速度**
```
dd if=/dev/zero of=/mnt/recorder/test.txt count=50 bs=64M
```
- /mnt/recorder为mount的目录
- 速度为200以上就正常
```
rm -rf test.txt
```
- 删除测试文件


**==DST/Dpro/Dpro2序列号确认方法==**
1. **telnet任意一个子板**
```
cat /etc/serial
```
- 有些设备确认不到


**==无法访问Manager网页，无法登录Viewer==**
1. **Ping Manager IP，确认是否能Ping通**
2. **确认VMS Manager服务器上Manager Service是否正常**
- 服务名: LG CNS Intelli-VMS Manager
- 重启一下该服务


**==Manager Service无法启动==**

1. **确认Postgre sql server服务是否正常启动**
- 服务名: postgresql-x64-9.3-PostgreSQL Server 9.3
- 如没有开启，启动该服务
2. **确认服务用的端口是否被其他进程占用**
- Task Manager→Details→manager_service.exe，确认该进程的PID号
```
netstat -ano | findstr 80
netstat -ano | findstr 443
```
- 确认80和443端口的进程是否是manager_service.exe的PID号，如果被其他进程占用，kill掉该PID号


**==CCTV画面无法显示==**
1. **确认该CCTV添加的Sub Board是否正常通讯**
- 重启该Dpro，还是不行更换Sub Board
 2. **确认Sub Board的receiver Log**
- telnet进该Sub Board
```
vi /data/log/receiver.log
```
- Connection Fail(0): CCTV连接状态异常→重启CCTV
- Connection Fail(4): CCTV信息与Dpro上登录的信息不同→确认Dpro上登录的CCTV的rtsp地址和id/pw
- Connection Fail(6): CCTV通讯异常→确认CCTV故障与否，线路故障与否


**==主板OS版本确认方法==**
- cat /etc/redhat-release


**==更换主板后，报错error: failure reading sector 0xc78 from 'hd0'，Entering rescue mode... grub rescue>==**
```
umount /dev/mapper/cl-home
fsck /dev/mapper/cl-home
```
- 出现什么都按y处理


**==更换主板后，无法mount到原有volumn==**
➣ 配置页上看不到volumn
➣ nic_setup.sh运行后eth的地址显示empty
➣ df -h 看不到mount的volumn
➣ systemctl status multipathd 有报错
➣ multipath -ll没有任何结果
1. **ip link show 确认eth0和eth2的mac地址**
2. **修改nic_setup.sh脚本后，运行+重启**
3. **如果还是看不到volumn，修改一下eth0和eth2的IP或者Mac地址(两个中随便一个)**
4. **eth0和eth2网卡重启一下(ifdown eth0; ifup eth0)**
5. **重启一下主板**


**==Viewer安装后，语言无法转换成中文==**
- 安装时一定要用管理者权限安装


**==关于DST/Dpro/Dpro2的DB==**
- 不同类型的设备之间不兼容
- DST与DST，Dpro与Dpro，Dpro2与Dpro2兼容


**==Dpro3联动相关==**
- VMS Manager 3.4.x.x以上才可以使用Dpro3


**==Manager搜索新dpro后，无法选择设备类型==**
1. **ssh进入主板**
```
/userdata/onvif/bin/configuration/cluster.xml
```
2. **找到ssl_on，把1改成0**
3. **重启dpro**


**==Manager上登录多个新dpro时uuid一样==**
1. **ssh进入主板**
```
/userdata/onvif/bin/configuration/cluster.xml
```
2. **找到并更改uuid**
3. **重启dpro**


**==DST/Dpro/Dpro2 Mode相关==**
➣ DST: 一旦选定了mode，此后更改mode，将会删除原有CCTV
- 例如：mode选了4HD，现在已经有2个HD的CCTV，一旦更改成其他mode，原有的2个HD的CCTV都会被删
➣ Dpro/Dpro2：mode里选择Automatic Transcoding，系统会根据添加CCTV的分辨率自动调节


**==PSIM权限==**
- 需要在[그룹-맵그룹]里添加역할权限
- 역할里的CCTV要包含该地图组里的CCTV
- 需要有管理者权限才可以编辑PSIM上的CCTV布局
★**管理者权限赋予方法：**
1. **登录VMS DB**
2. **找到tb_role表，找到需要赋予管理者权限的role名**
3. **使用set方法设置**
```
update tb_role
set admin_yn = 'y'
where role_name = '要赋予权限的role名'
```


**==PSIM/VMS Viewer登录权限==**
- Manager网页设置[사용여부]为No，登录Viewer时==提示账号或者密码不正确==
- Manager网页设置[허용된 Viewer IP]，登录Viewer时==提示不是许可的IP==


**==EVM/VMS 网页/Viewer登录问题==**
- 网页输入账号密码，提示账号或密码错误
- Viewer也一样，即使能登进去，无法刷新出CCTV List或者无法播放画面
1. **重启服务器**


**==PoE标准==**
➣ PoE
- 802.3af
- 供电功率：12.95～15.4W
- 供电对数(网线)：2对
- 适用：VoIP电话、无线AP、摄像头

➣ PoE+
- 802.3at
- 供电功率：25～30W
- 供电对数(网线)：2对
- 适用：PTZ摄像头、视频电话、AP

➣ PoE++
- 802.3bt Type3
- 供电功率：51～60W
- 供电对数(网线)：4对
- 适用：视频会议系统、多射频AP

➣ 4PPoE
- 802.3bt Type4
- 供电功率：71～90W
- 供电对数(网线)：4对
- 适用：笔记本电脑、大型显示器


**==子板ftp问题==**
➣ 使用alzip或者从其他子板使用curl命令时报错
➣ 其他子板telnet该子板报错，该子板telnet其他子板正常
1. **/etc/init.d目录下确认start_app是否异常或无内容**
2. **从其他子板复制粘贴start_app内容到该子板**
3. **重启子板**


**==Dpro启动时无限循环进入BIOS界面==**
1. **确认SSD连接线有无问题**
2. **更换SSD**


**==Viewer提示Login failed for stream user==**
➣ 通常发生在更换子板后
1. **确认和其他子板的固件版本是否一致，如不一致则统一**
2. **确认/etc/init.d目录下的start_app和static_arp.sh是否有运行权限，如无则赋予运行权限**
```
cat /userdata/onvif/bin/configuration/user.conf
```
3. **是否和其他子板一样，如不一样则统一**


**==关于Dpro/Dpro2/Dpro3的主板子板兼容==**
- Dpro/Dpro2兼容，通过升级或降级FW版本
- Dpro3和Dpro/Dpro2硬件上不同，不兼容


**==Dpro子板与CCTV通讯确认==**
1. **telnet连接到该子板**
2. **子板上运行如下命令**
```
telnet 192.168.0.10 554
```
- 554为rtsp端口
3. **Ctrl+C取消telnet**
- 立马取消且无结果: 554端口不通
- 出现Console escape: 554端口通


**==Dpro3相关==**
- putty进去时，不能直接用root，要先用admin进去，然后切换到root
- admin / nexreal2013
```
sudo su -
```


**==永和仓库NVR信息==**
- 从左到右:16路，16路，32路
- IP: 10.25.2.253 / 252 / 254
- 登录账号:admin / gzLG2022@
★ **萤石云账号** 
- 13316086560 / LGDhilo12


**==CCTV视频卡顿==**
➣ VMS Viewer上的实时/录像卡顿，导出的录像卡顿不平顺
➣ Local PC播放正常，Cloud PC播放有此现象
- Cloud PC为VM分配的显卡，显卡性能低下导致画面不顺畅


**==特定dpro在Viewer上无录像进度条==**
➣ dpro网页上无法看到存储，df -h也无法看到
➣ iscsiadm -m session可以看到有iscsi连接
➣ 运行/recorder/mount-storage.sh
	报错:failed: Structure needs cleaning
1. **运行文件系统检查修复**
```
fdisk -l
```
- 确认volume路径，如/dev/mapper/mpathc
2. **运行下面命令修复**
```
xfs_repair /dev/mapper/mpathc
```
3. **2失败，运行下面命令**
```
xfs_repair -L /dev/mapper/mpathc
```


**==PSIM Viewer上部分楼层图纸不显示==**
➣ CCTV能正常显示
★ PSIM Viewer安装包里缺少该楼层图纸导致
1. **从Manager服务器把缺失的楼层的图纸粘贴到本地**
2. **登录PSIM Viewer**
3. **点击图纸缺失的页面，导入1中的图纸**


**==光纤收发器==**
![[1761033192025_323.jpeg]]


**==CCTV PW变更相关==**
➣ CCTV的PW变更后，批量变更Dpro的sqlite上的CCTV PW后，Viewer上出现部分CCTV黑屏(能Ping通)
1. **Dpro网页上重启该Dpro**
2. **用批量重启CCTV的python脚本重启该Dpro上的CCTV**


**==Manager网页上更新Dpro的变更信息Issue==**
➣ 批量变更Dpro上的CCTV PW后，在Manager网页上的[카메라 가져오기]上一键更新时，网页==报错502ok==，Viewer登录时提示无法连接到Manager
★ **批量更新导致VMS Manager服务Down**
1. **服务里确认VMS服务相关服务是否开启，如没有开启则开启**
- 不要几千个一起弄!!!


**==摄像头彩页==**
➣ EV Dome
[[2CD2526FWD.pdf]]

➣ Box
[[DS-2CD7A27EWD系列.pdf]]

➣ Dome
[[DS-2DE2204IW.pdf]]

➣ Speed Dome
[[iDS-2DF8432IXR-A(T5).pdf]]

➣ WiFi CCTV(Axis)
[[M1045-LW datasheet.pdf]]

