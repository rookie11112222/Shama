**==持续Ping，显示时间，结果保存到txt文档==**
1. **Win + R**
2. **填写powershell**
3. **ping.exe -t IP | Foreach{"{0} - {1}" -f (Gdt-Date),$\_} > D://1.txt (结果保存到D盘的1.txt里)**


**==快捷打开Windows Scheduler==**
- 运行 - taskschd.msc


**==确认PC的基本配置==**
- cmd - dxdiag


**==确认CPU个数==**
- cmd - systeminfo 
- 确认处理器部分


**==确认CPU的Core数，线程数==**
- cmd - wmic cpu get NumberOfCores, NumberOfLogicalProcessors
- NumberOfCores : 核心数
- NumberOfLogicalProcessors : 线程数


**==NTP设置方法==**
- 连接AD的服务器除外
1. **SpecialPollInterval 设定(60分)**
	➣ a. regedit修改方法         [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\W32Time\TimeProviders\NtpClient] 新登录或修改     SpecialPollInterval 十进制后更改为3600
	➣ b. CMD修改方法
	Reg add ''HKLM\SYSTEM\CurrentControlSet\services\W32Time\TimeProviders\NtpClient'' /v SpecialPollInterval /t REG_DWORD /d 3600 /f
2. **Service Stop & Start**
- net stop w32time
- net start w32time
	2-1. 运行[net start w32time]时报错
<错误内容>
发生系统错误 1058
无法启动服务，原因可能是已被禁用或与其相关联的设备没有启动

---
1. services.msc找到Windows Time服务，如果禁用，改为自动
2. cmd输入 w32tm /register
---

<错误内容>
发生系统错误 5
拒绝访问
- 搜索cmd，右键[以管理员身份运行]
  
 3. **PC重启时自动Startup设定(cmd)**
- sc config w32time start= auto
- sc triggerinfo w32time start/networkon stop/networkoff
4. **其他设定**
- w32tm /config /syncfromflags:manual /manualpeerlist:''10.72.165.38,0x9'' /update
- 上面的IP是NTP服务器的IP，咨询各自法人的网络担当者
5. **Resync**
- w32tm /resync
6. **确认设定状态**
- w32tm /query /status
- w32tm /query /peers
- w32tm /query /configuration
- w32tm /dumpreg /subkey:parameters
- sc query w32time


**==PC更改时区后的Log==**
1. **进到事件查看器→Windows日志→系统**
2. **会有[系统时间已从xxxx 更改为 xxxx。 更改原因：System time adjusted to the new time zone]等Log**


**==CO时间服务器==**
- Linux : 10.72.165.37 
                10.72.165.38
- Windows : 10.72.128.43


**==查看服务器序列号==**
➣ cmd下运行以下3个中的一个
- wmic bios get serialnumber
- wmic csproduct get name, identifyingnumber
- wmic csproduct (显示信息非常全)


**==隐藏文件/解除隐藏文件==**
➣ cmd命令行中运行
➣ 隐藏文件
- attrib +s +h ＂文件目录＂
➣ 取消隐藏文件
- attrib -s -h ＂文件目录＂


**==cmd下重启/关机命令==**
➣ 重启(倒计时1分钟)
- shutdown /r
➣ 立即重启
- shutdown /r /t 0
➣ 关机(倒计时1分钟)
- shutdown /s 
➣ 立即关机
- shutdown /s /t 0


**==无法连接OA无线==**
➣ 其他电脑可以
1. **确认日期和时间是否正确，如不正确重新设置**
- 时间不正确会导致无法连接无线
 2. **重启电脑后，再连接无线**
- 还不行的话，需要IT修理室更新无线证书
- AD连接的电脑是自动NTP校时的


**==单位换算==**
➣ MB：单位以10为底数的指数
- 例子：1KB=10^3 =1000,
- 1MB=10^6=1000000=1000KB
- 1GB=10^9=1000000000=1000MB
➣ MiB：是以2为底数的指数 
- 例子：1KiB=2^10=1024,
- 1MiB=2^20=1048576=1024KiB
- 1GiB=2^30=1,073,741,824=1024MiB


**==查看显卡型号==**
- lspci | grep -i vga
- 会输出4位的16进制代码，到特定网站搜索


**==查看显卡信息命令==**
- nvidia-smi
![[20220407145735.png]]
- 第一栏的Fan：N/A是风扇转速，从0到100%之间变动，这个速度是计算机期望的风扇转速，实际情况下如果风扇堵转，可能打不到显示的转速。有的设备不会返回转速，因为它不依赖风扇冷却而是通过其他外设保持低温（比如我们实验室的服务器是常年放在空调房间里的）。
- 第二栏的Temp：是温度，单位摄氏度。
- 第三栏的Perf：是性能状态，从P0到P12，P0表示最大性能，P12表示状态最小性能。
- 第四栏下方的Pwr：是能耗，上方的Persistence-M：是持续模式的状态，持续模式虽然耗能大，但是在新的GPU应用启动时，花费的时间更少，这里显示的是off的状态。
- 第五栏的Bus-Id是涉及GPU总线的东西
- 第六栏的Disp.A是Display Active，表示GPU的显示是否初始化。
- 第五第六栏下方的Memory Usage是显存使用率。
- 第七栏是浮动的GPU利用率。
- 第八栏上方是关于ECC的东西。
- 第八栏下方Compute M是计算模式。
- 下面一张表示每个进程占用的显存使用率。


**==网线线序(标准568B)==**
- 橙白/橙/绿白/蓝/蓝白/绿/棕白/棕
- 从左到右
- 1/2/3/6为数据发送接收，4/5/7/8供电或者电话线用的


**==服务器解除禁ping方法==**
1. **控制面板→防火墙**
2. **中间防火墙全部设置为关闭**


**==服务器分区设置S2O报警相关==**
- 可为一个服务器的多个分区分别设置报警阈值
- 如无特殊要求，一般同一个服务器的所有分区都使用同样的报警阈值


**==radmin离线激活==**
- 取得request文件后，一定要在PC端输入request文件后取得license文件
- 用手机取得的license文件会有损坏，下载前显示为500B，下载后实际为100多B
- radmin离线激活网站：http://activate.famatech.com/upload.htm


**==radmin连接异常==**
➣ 连接时出现电脑图标，但无法连进去
1. **删除原来安装的server软件**
2. **重装server软件后，重新连接**

