**==Vivace Manager登录输入源**==
➣ PC
- admin / 1111
- 1111

➣ CCTV
- stream / 1111
- root / 1111


**==CO CMS室 Encoder地址==**
➣ Notebook
- rtsp://10.72.205.54/video1
- admin / 1111
➣ VideoConference
- rtsp://10.72.205.55/video1
- admin / 1111


**==DID型号==**
- CO CMS : 49VL5B(LGE) 
- CO CCCG / CA CC : 49VL5G(LGE)
- CA CMS : LD470DUN(屏幕)+DH470LGM1(拼接控制器) 自行拼装的 控制器厂家:DIMACRO
- CO CMS和CO CCCG / CA CC的可以互换，但因为有色差，不建议


**==Controller 显卡==**
- CO CMS / CO CCCG / CA CC : image2K    
- CA CMS : DP4 
- 不兼容


**==视频编码器与视频解码器==**
- 视频编码器(Encoder) : 模拟信号转换成数字信号 + 压缩 (输入)
- 视频解码器(Decoder) : 数字信号转换成模拟信号 + 解压缩 (输出)


**==Controller 华硕主板 Q Code==**
![[mmexportd7317fd49beb54749138eb5c4c0f4e11_1676429774171.jpeg]]
![[mmexport3abc0455d1cfa67eec22484fb3b9df49_1676429781418.jpeg]]
![[mmexport3d8af2e021b747a52e3d25fad4181391_1676429777285.jpeg]]


**==CO CMS室 IP Wall信息==**
![[IMG_20231130_114654.jpg]]

- Controller #1 : 左边 4\*12
- Controller #2 : 右边 4\*12

➣ DID排序(屏幕基准)
- 上面的图

➣ DID排序(Controller基准)
- 各自管理区域的左上角开始
- 1-6 / 7-12 / 13-18 / 19-24

➣ DID(DVI) - Controller(DVI to HDMI)

➣ KVM账号密码
- 无账号密码，确认即可进入


**==CMS室内设备IP==**
➣ CO
- Controller #1 : 10.72.205.51  
- Controller #2 : 10.72.205.52
- MCS PC : 10.72.205.53 
- VMS Viewer PC : 10.72.205.56 -79


**==CO 信息保安监控室==**
➣ VMS Viewer PC
- 10.72.205.31-36
- 每台PC控制两个屏幕
➣ 一般VMS Viewer PC
- 10.72.205.41
- 用于看回放和PSIM


**==CO KVM管理界面里1号到8号 VMS PC键鼠失灵==**
1. **重启1号到8号连接的KVM Switch**
2. **变更主KVM Switch的端口(9转11)**


**==CO CMS室左边半边屏幕闪烁掉线==**
➣ 该Controller Ping掉包
➣ 网线/交换机端口没问题
1. **Controller网卡有问题，更换其他网卡端口恢复(有2个网口)**


**==Vivace上某一sub无法投屏(投屏没反应)==**
➣ EDID无问题
1. **KVM进入无法正常投屏的PC**
2. **NVIDIA控制面板-设置多个显示器-两个都勾选**
- 正常情况下，配置Surround, Physx界面里两个都要显示绿色


**==KVM显示器进入18/19PC黑屏==**
➣ 有信号但黑屏，可以看到鼠标移动
➣ Vivace上18/19的sub无法投屏
➣ 可以在其他PC mstsc进这两台PC
★ 这两台PC上的NVIDIA控制面板上显示不全(只有3D设置，其他的都不显示)
1. **重新安装显卡驱动(手机里有)**
2. **重启电脑+插拔显卡上连接的所有线**


**==某一个屏幕花屏==**
➣ 拔出信号线，正常显示蓝屏
1. **必须要先重启看看能不能恢复**


**==Encoder设定==**
1. **把将要和Encoder连接的PC设为和Encoder初始IP同网段IP**
- Encoder初始IP: 192.168.10.100
2. **网线直连PC和Encoder**
3. **网页进入Encoder的配置页**
- 初始账号密码: admin / leadtech21
4. **设置LGD IP**
- 配置页右上角Setup - 左侧Nework - IP&Port里设置IP
5. **Encoder连接**
- Encoder网口接交换机，需要投屏的PC接Encoder的HDMI IN接口
6. **VIVACEManager设置**
- MCS PC运行VIVACEManager，用Admin账号进入，左下角IP Camera里添加Encorder
- IP地址: rtsp://192.168.0.45/video1
- 192.168.0.45为Encoder IP
- ID / PW : admin / leadtech21
7. **拖到中央位置，确认Wall上能否正常显示**
8. **MCS软件设置**
	8-1. 进入MCS软件的安装路径(C:\MCS)
	8-2. 编辑打开LGDGuangzhouP10AMCS.bat文件(文件名可能不一样，修改配置前必须备份！)
	8-3. //TAB2下寻找空白处依次填写Encoder名称(VIVACEManager上定义的名字)
	8-4. 运行MCS软件
	8-5. AGENT INPUT(右上角) → ENCODER里可以看到添加的Encoder
	8-6. 鼠标左键长按(约2S)新增的Encoder，会出现小弹窗
	8-7. 确认并填写小弹窗里的信息  
	- 格式: MCS软件上显示的按键名//Encoder添加在VIVACEManager上的路径//Encoder添加在VIVACEManager上的名字
	- 例如Viewer_30//IPCAM//Viewer_30


**==多个屏幕时，当前屏幕为某一副屏，[显示设置]在主屏上==**
1. **右键点出[显示设置]，这时显示设置应该会显示在主屏幕上**
2. **下面任务栏鼠标点击选择[显示设置]后，按Alt + 空格**
3. **点击M键**
4. **鼠标光标消失: 长按左或右方向键**
5. **鼠标光标没消失: 多次点击Win + 左或右方向键**
- 具体按左还是右，根据显示器的分布定，一般按左


**==特定VMS PC无法投屏==**
➣ DID无问题，拖动其他PC到该DID上能正常投屏
1. **KVM切换进该PC**
2. **电脑右下角确认VivaCe Agent Manager软件有无运行，没运行就运行起来**
3. **右键该软件点击Setting, 确认[Select Monitor]，要设成[Simul.Dual Monitors]**


**==Vivace Agent上的Select Monitor设定==**
➣ 是指需要投屏的PC上安装的Agent软件
- Simul. Dual Monitors
- CA CMS / CA CCSS / CO CMS / CO CCSS / CO CGSS都一样


**==CO CCSS==**
➣ Controller
![[IMG_20250306_111111.jpg]]
➣ DID
![[IMG_20250306_111128.jpg]]
➣ HDMI延长器(左到右)
![[IMG_20250306_111052.jpg]]


**==部分DID一闪一闪黑屏==**
➣ 特定某个DID一闪一闪，闪烁周期几秒到几十秒不定
1. **在Controller端将正常输出的HDMI线和异常输出的HDMI线对调，确认是否为显卡输出问题**
2. **如确认为显卡输出异常，先在vivace manager上clear屏幕后重新投屏**
3. **第二步未能解决的话，重启一下Controller**


**==MCS上批量登录CCTV教训==**
- 提示csv文件破损的不要upload
- 会出现MCS软件上无法连接Controller的问题，会提示connection refused
- 重启两个Controller才解决


**==拼接屏控制器==**
➣ 分配器 
- 一进多出
- 输入:电脑
- 无开窗漫游等功能
- 应用场景:报告厅，展厅，会议室

➣ 矩阵
- 多进多出
- 输入通常是4的倍数
- 输入:电脑
- 应用场景:报告厅，展厅，会议室

➣ 解码器
 - 多进多出
 - 输入通常是4的倍数
 - 输入:电脑/监控摄像头
 - 部分有开窗漫游等功能
 - 应用场景:监控室，指挥室，调度室

➣ 图像处理器/拼接处理器/外置拼接处理器(外控)
 - 多进多出
 - 输入通常是4的倍数
 - 输入:电脑/监控摄像头
 - 任意开窗漫游
 - 板卡形式
 - 应用场景:监控室，指挥室，调度室

➣ 分配器 -> 矩阵 -> 解码器 -> 处理器