**==历史数据查询期间==**
- 历史数据: Max 7天
- 历史趋势图: Max 2个月


**==Server/DB连接信息==**
➣ 服务器
- UMS: ums_adm01  /  !qaz2wsx
- UMS IF : xappcyx / Wcyx1990$

➣ DB
- UMS: NeconServer  /  -pl,mko0
- Local PC: system
- IF ServerDB : GNCSDB / GNCSDB

➣ FTP
- ftp://10.72.134.34/
- ums_ftp01  /  !qaz2wsx


**==防火墙端口==**
- 21/8676/1234/2345/3433
- OA Client里8676 telnet不通


**==Local PC ODBC Setting==**
- ID : NeconServer
- PW : -pl,mko0
- 端口: 3433


**==PLC端口==**
- ETOS/GLOFA PLC : 2004
- Modbus : 502


**==PLC信息==**
- XGT: LS产电
- HC900: Honeywell
- MELSEC: 三菱(MITSUBISHI)
- ETOS: AC&T
- AB: ROCKWELL 


**==PLC IO地址==**
- %MW: PLC的存储单元，常用于储存数字信号、自然数和浮点数等数据类型。MW是一个16位的单元存储器。
- %MW10.4为一个字，一个字分为2个字节，一个字节8个位，也就是说%MW10中有16个位，从%MW10.0~%MW10.15


**==管制点NPBC==**
➣ NNPPBBCC，一共8位
- NN : Network编号(各System的识别编号)
- PP : PLC编号(Multiport编号)
- BB : PLC Board编号(PLC I/O Module编号)
- CC : PLC Point编号(Channel编号)


**==ETOS==**
- Ethernet TO Serial Gateway
- 与PLC类似，但比PLC低端，一般只用于数据传递和简单的控制


**==名词解释==**
- VFD: Variable-frequency Drive 变频器
- FMS: 温湿度
- DMS: 颗粒物
- LT알람: Local Temp
- SA알람: System Alarm
- POU: Program Organizational Unit
- CV: Control Valve (控制阀)
- SV: Shut-off Valve (关断阀)
- HV: Hand Valve (手动阀)
- PV: Process Variable (过程变量) → 当前值
- SP: Set Point (设定点)
- MSP: Mininum Summer Set Point (夏季最低设定点)


**==DI Local电脑==**
- 有两台Local PC
- 一台直连现场PLC，能控制现场设备，也能接收数据
- 另一台不连接现场PLC，联网，只能接收数据不能控制
- 只有DI才这么特殊


**==UMS Local PC时间同步相关==**
- 实际时间比PC时间慢的时候，同步后等一段时间(快多少就等多少)
- 实际时间比PC时间快的话，不用等直接启动就行


**==现场PC上的MAS设置被变更时==**
1. **去UMS软件路径，找到Cms.exe，确认修改时间，如果是10/18左右就表示软件没有变更就比较好办**
2. **进入Config文件夹，再进入该工种的文件夹(如再利用:P10A-REUSE)，打开P10A-REUSE.ini文件**
3. **找出DbDataDll项，确认后面的为DbPgSqlMas.dll，如果不是改成这个**
4. **PlugIn文件夹下可以找到DbPgSqlMas.dll文件，复制粘贴他的名字即可**
5. **重启软件后，依次点击工具-同步(Upload)，工具-同步(Database)，等待一段时间后确认数据**


**==本地电脑没有历史数据==**
➣ OA Client上正常，就本地电脑不行
➣ 本地电脑:SystemQuery:1 / ServerQuery:0
➣ 服务里手动启动失败，应用Log显示服务启动超时
★ 本地pg数据库未启动，C盘(数据库安装盘)硬盘容量不够
1. **删除pg_log目录下的之前的文件(该目录为数据库运行日志，删除不影响服务，关闭数据库服务状态下删除)**
2. **确保充足空间 + 杀掉所有pg数据库相关进程后，重新启动pg服务**


**==本地电脑postgre服务无法启动==**
➣ postgre安装在C盘，C盘空间够
➣ 事件查看器里显示等待服务超时
1. **taskmanager里kill掉所有postgre相关进程**
2. **cmd后进入postgre的bin目录(用cd命令)**
3. **运行下面命令** 
```
.\pg_resetxlog.exe -f ..\data
```
4. **如果提示postmaster.pid已经存在就删除他，路径为postgre安装路径下的data下**
5. **启动服务**


**==SQLSERVER 查询某个时间段的数据==**
```
select * from 表名 
where substring(convert(varchar(50),Column名,120),1,10) = '2024-01-12'
```


**==历史报警下载excel报错==**
➣ 提示AlarmHistory.xls不存在
1. **安装路径下确认有无AlarmHistory.xls文件**
2. **去别的地方复制粘贴一个到安装目录里(其他的OA Client里复制粘贴即可)**


**==热源班现场设备构造==**
- 锅炉厂家电脑 - Melsec PLC - ETOS
- 中间都是RS485连接


**==Postgre_Local DB Setup==**
1. **安装Postgre**
- posesql-9.5.3-1-windows.exe
- Password设为system
2. **DB生成**
	  2-1. 生成DB文件夹
	- CreateDB_FORDER.bat
	 2-2. 运行pgAdmin III软件
	- 开始菜单下找一下
	 2-3. 双击左侧PostgreSQL9.5 (localhost:5432)，输入密码 system
	 2-4. 选择左侧postgres后，点击中间 SQL 模样的按钮
	 2-5. 在中间编辑窗口拖拽sql语句
	- 1.Create_Database.sql 
	2-6. 全选出现的sql语句，点击中间带PGS的播放按钮(不是播放按钮，而是带PGS的播放按钮)
	 2-7. 回到pgAdmin III，左侧选中PostgreSQL 9.5(localhost:5432)后，点击左上角刷新按钮，确认有没有生成DB
	 2-8. 回到sql语句编辑窗(第五步)，中间找到[postgre在 postgres@localhost:5432]的字样，点击旁边下拉键，点击<新连接>
	 2-9. 按照如下选择
	- 服务器: localhost:5432
	- 数据库: NeconLocal
	- 用户名: NeconLocal
	- 角色名称: NeconLocal 
	 2-10. 点击确定后输入密码
	- -pl,mko0 
	 2-11. 用NeconLocal登录后，在中间sql编辑窗拖入sql语句后运行
	- 3.Create_Function.sql
	 2-12. 确认是否正常生成Function(同第7步)
3. **Windows防火墙Open端口(win7为例)**
	3-1. 控制面板→Windows防火墙→高级设置(左边)
	3-2. 左边选择[入站规则]后，右键点击[新建规则]→选择[端口]→选择[TCP]和[特定本地端口]，端口写5432后点击下一步
	3-3. 选择[允许连接]后下一步
	3-4. [域]，[专用]，[公用]都选择后下一步
	3-5. 名称写[POSTGRE_PORT]后，点完成
	3-6. 按照上面步骤，出站规则也弄一遍


**==DB Move(同一PC，移动到不同路径)==**
1. **D盘创建CmsLocalDB文件夹**
2. **运行pgAdmin III软件，左侧TableSpaces\NeconLocal鼠标右键，点击[New TableSpace...]**
3. **上面[Properties]，中间Owner选择NeconLocal**
4. **上面[Definition]，Location选择第一步建立的路径**
5. **上面[SQL]，勾选[Read Only]，下面编辑区写**                                             
```
CREATE TABLESPACE "NeconLocal2"
OWNER "NeconLocal"                LOCATION  D:\\CmsLocalDB';
```
6. **填写完上面3-5步后，点击OK，确认Tablespaces上是否生成了NeconLocal2**
7. **在原有的Tablespace，也就是NeconLocal右键点击[Move objects to...]**   
8. **弹窗如下设置后点击Ok                         New tablespace  --  NeconLocal2           Object's kind  --  All                                     Object's owner  --  NeconLocal**
9. **在原有的Tablespace，也就是NeconLocal右键点击[Properties...]**
10. **弹窗上面[Definition]，中间Tablespace选择NeconLocal2**
11. **paAdmin III里选择Database - NeconLocal，中间上面选择Properties，中间的Tablespace和Default tablespace显示NeconLocal2，且第一步生成的文件夹里正常生成文件就表示成功**


**==DB BackUp & Restore(移动到其他PC)==**
1. **运行pgAdmin III软件**
2. **双击PostgreSQL 9.5(localhost:5432)，输入密码 system**
3. **Database\NeconLocal右键点击[Backup...]**
4. **弹窗下面[File Options]→Filename选择要备份到的路径以及名字**
5. **弹窗下面[Objects]→全选后，点击[Backup]按钮**
6. **备份成功，最后一行会显示[Process returned exit code 0.]，点击[Done]按钮**
7. **在需要Restore的PC上运行pgAdmin III**
8. **双击PostgreSQL 9.5(localhost:5432)，输入密码 system**
9. **打开到Database\NeconLocal\schemas\public\Tables(0)                                                  Tables(0)表示Table数量为0**
10. **NeconLocal 右键点击[Restore]**
11. **弹窗下面[File Options]→Filename选择备份文件所在路径，Rolename选择NeconLocal，选择完后点击[Restore]按钮**    
12. **Restore需要一定时间，Restore成功会提示[Process returned exit code 1.]，成功后点击[Cancel]**
13. **确认Tables里是否正常出现table**


**==Local电脑设置==**
★ Cms.ini
[DB_INST]
- AutoDsnInst=0  (需设定自动DSN时，设为1)
- RemoteDsn=0
- OracleInst=0 (设定Oracle自动DB时，设为1(坡州，越南))
[DB_SETUP]
- SystemSave=1 (Local DB存数据)
- SystemQuery=1 (Local DB查数据)
- ServerSave=1 (Server  DB存数据)
- ServerQuery=0 (Server DB查数据)
	➣ SystemQuery和ServerQuery只能一个设为1

★ P10A-SCR_VOC.ini
[SYS_LOCAL]
- Count=2 (有几个N就填几个，NPBC的N)
- SYSID_01=59 (Local N的编号)
- NAME_01=P10A-SCR_VOC (Local Name)
- TAGDB_01=P10A-SCR_VOC-PT.NCS (Local Name - PT.ncs)
- IP_01=10.72.136.232 (Local PC IP)
- ODBC_01=NeconLocal
- SYSIO_01=P10A-SCR_VOC
- DSNIP_01=(LOCAL)
- DSNID_01=NeconLocal
- DSNPW_01=-pl,mko0
- ALIAS_01=P10A-SCR_VOC
	➣ 另一个Local N参照上面生成，其他都一样，SYSID_02设成其他的Local N
[Link_Dll]
- DbRawPtDll=DbLRawPtAcs.dll
- DbMapPtDll=DblMapPtAcs.dll
- SocketDll=SockLocalCH.dll
- DbDataDll=DbPgSqlMas.dll (CO)


**==postgre确认 DB Size==**
1. **pgAdmin III页面左侧选择Tablespaces**
2. **中间部分点击Statistics**


**==postgre确认DB Status==**
1. **运行pgAdmin III，连接数据库，密码为system**
2. **点击上面[Tools]→Server Status**
3. **右边Logfile窗口可以确认内容**


**==postgre删除Table后，DB Size无变动==**
➣ 删除table后，DB Size无变动或有其他问题时，可以使用vacuum verbose analyze命令操作
➣ 简单的vacuum会有效果，如无效可尝试full vacuum，full vacuum时会使用大量DB资源，有可能无法正常保存/查询History，建议关闭MMI后操作
1. **运行pgAdmin III，点击[SQL]按钮**
2. **输入下面命令，确认一下DB有无开启autovacuum功能，结果显示on表示功能已开启**
```
show autovacuum
```
3. **简单vacuum**
```
vacuum verbose analyze
```
4. **full** **vacuum**
```
vacumm full analyze
```


**==数据迁移方法==**
➣ Local电脑重装OS后，将UMS DB上的原有数据迁移到Local电脑
★ **方法1**
- Sqlserver → PostgreSQL → PostgreSQL Backup → PostgreSQL Restore
1. **Postgre安装及DB生成(参考上面的内容)**
2. **控制面板→管理工具→数据源(ODBC)→PostgreSQL Unicode**
3. **弹窗内容如下设置:**
- Data Source: NeconLocal              
- Database: NeconLocal     
- Server: 127.0.0.1          
- User Name: NeconLocal        
- Password: -pl,mko0
4. **设置完后，点击[Test]按钮，确认成功与否**
5. **运行NCDBMngr.exe，移动Table**
6. **左上角点击[Transfer DB]**
7. **点击左侧Source，输入UMS Server DB的信息后，点击[Connect]按钮**
8. **点击右侧Target，输入Local DB的信息后，点击[Connect]按钮**
9. **选择System和中间的时间后，点击中间[Transfer]按钮**
10. **利用Postgre的Backup功能，进行Backup(参照上面)**
11. **利用Postgre的Restore功能，进行Restore(参照上面)**

★ **方法2**
- Sqlserver → CSV 文件 → PostgreSQL
1. **在UMS Server上利用Sqlserver导出数据工具导出table和数据，格式设定为CSV**
2. **编写用于导入Local电脑Postgre数据库的脚本**
```
CREATE TABLE "H201908_P7_HVAC_010101" ("DTIME"  TIMESTAMP, 
"P01"  FLOAT, 
"P01S"  INT, 
....  
"P99S" INT);

ALTER TABLE "H201908_P7_HVAC_010101"
ADD CONSTRAINT PK_H201908_P7_HVAC_010101  PRIMARY KEY ("DTIME")
```
3. **用编写好的脚本进行CSV文件的Loading和Restore作业**
```
COPY H201908_P7_HVAC_010101 (DTIME,P01,P01S,.....,P99S) FROM D:\BACKUP\H201908_P7_HVAC_010101.CSV DELIMITER,  CSV HEADER;
```


**==方法1中ODBC数据源没有PostgreSQL==**
1. **PostgreSQL官网下载ODBC驱动**
2. **安装**
- 手机业务文件文档里有


**==FMS温湿度点位↔DES联动==**
1. **FMS Local PC上确认需要联动点位的NPBC信息**
2. **驱动程序→DES点设置**
3. **点击左侧[00:温度/湿度]和[01:温度/湿度]，中间部分往下拉找到空白位置，点击空白位置上面一行后，点击空白行**
4. **点击上面[INDEX]旁边[..]按钮，弹窗上面选择[Number]，选择要添加的温度点位的NPBC**
5. **更改INDEX下面的信息(PDE班提供)**
6. **重启UMS软件**
- [00:温度/湿度]  --  温度
- [01:温度/湿度]  --  湿度


**==SPC数据相关==**
- INV/TEMP数据: 每分钟上传一次(历史数据)
- 详细工种里的数据: 每小时上传一次(历史数据)，每个时间段的1分上传，例: 8:01 / 9:01 …等


**==SPC SCADA数据长期holding==**
- 有实时数据，但长期holding，holding期间不定，有可能是1天，也有可能是几天
1. **重启PC**


**==Config文件相关==**
- IO.ncs : IO信息相关数据
- MAP.ncs : 画面信息相关数据
- PT.ncs : 管制点属性信息相关信息
- 打开上面的需要用Access


**==PT.ncs 更改相关==**
- 直接删除表，导入表数据 ：重启UMS软件后恢复成原来数据
- 打开PT.ncs后，编辑修改：会保留修改后的值，重启UMS软件不会恢复成原来数据


**==Modbus通讯协议==**
★ 01 - Read Coils(线圈) - 0x
- 11(协议地址)→10(第10个)
- 从1开始计数

★ 02 - Read Discrete Inputs(离散输入) - 1x
- 10011(协议地址)→10(第十个)
- 从10001开始计数

★ 03 - Read Holding Registers(保持寄存器) - 4x
- 40011(协议地址)→10(第十个)
- 从40001开始计数

★ 04 - Read Input Registers(输入寄存器) - 3x
- 30011(协议地址)→10(第十个)
- 从30001开始计数

★ 05 - Write Sing Coil

★ 06 - Write Single Register

★ 15 - Write Multiple Coils

★ 16 - Write Multiple Registers


**==OPC通讯协议==**
- OPC : OLE for Process Control
- OLE : Object Linking and Embedding，微软为Windows系统、应用程序间的数据交换而开发的技术
★ Siemens PLC
- OPC软件 : OPC.SimaticNET(西门子提供)
- OPC软件 : KEPServerEX(LGD在使用)


**==OA Client里添加新的工种的方法==**
➣ 前提条件：实际有该工种的UMS Local PC，相关Config文件也会上传到UMS AP/DB服务器的FTP文件夹里
➣ 以添加FMS工种为例
1. **FTP文件夹里下载FMS文件夹里的D1-FMS.ini文件**
2. **打开D1-FMS.ini文件，确认[NAME]和[IP]**
3. **OA Client路径里新生成[CLIENT-P-FMS.ini]文件(可复制其他的ini文件)**
4. **更改[CLIENT-P-FMS.ini]文件内容：[CLIENT_SETUP]里的DefLocal输入第二步确认到的NAME，MaxLocal填写1，[LOCAL_LIST]里的Local和IP分别填写第二步确认到的NAME和IP**
5. **更改SmartClient.ini文件：[MAXSYSTEM]更改为添加后的工种数量，下面依次填写需要添加的工种内容**
- BTN17=CLIENT-P-FMS, FMS, 360, 390, 500, 420,, D1-FMS
- CLIENT-P-FMS为第三步生成的ini文件名
- FMS为OA Client里选择工种页面上需要显示的工种名
- 360, 390  工种选择页面里FMS工种方框的左上角的坐标
- 500, 420  工种选择页面里FMS工种方框的右下角的坐标
- D1-FMS  第二步里确认到的NAME
6. **更改工种选择页面里的抬头，Main-CAD1.bmp**
7. **连接测试**


**==가상IO에 관제점 추가하는 방법==**
1. **변경 원하는 가상IO에 마우스 우클릭→[통신(I/O)수정]**
2. **상단 오른쪽 첫번째 DELETE를 눌러 Mapping된 가상포인트를 삭제한다**  
3. **삭제 후 밑의 [디바이스 정보]의 [가상 포인트 기기 번지]와 [가상 포인트 명]이 비여진걸 확인한다**
4. **상단 왼쪽 두번째 EDIT를 누른다** 
5. **오른쪽의 관제점 리스트에서 추가 원하는 관제점을 더블 클릭하면 중간에 추가된게 확인된다. 오른쪽 관제점 리스트에 추가할 관제점이 없을 경우 오른쪽 하단의 [I/O 연결 관제점]을 선택하고 찾는다**
6. **밑의 EDIT를 누르고 EXIT를 눌러서 나간다**
7. **상단 왼쪽 네번째 [ADD] 누른다**
8. **나오는 창의 오른쪽 관제점 리스트에서 step 2에서 삭제한 포인트를 찾고 선택한 후 하단의 ADD를 누르고 EXIT를 누른다**
9. **가상 포인트 설정 화면에서 관제점이 추가된것을 확인할수 있다**
10. **도구→동기화(Upload)와 동기화(Database)를 해준다**
- 가상포인트 설정화면 상단에 ADD/EDIT/DELETE가 두셋트 있는데 앞의꺼는 가상 디바이스에 대한 설정이고(왼쪽), 뒤의꺼는 가상포인트에 대한 설정이다(디바이스 정보)



**==System构成相关==**
➣ 构成状态
- CO/CA: C++, MFC Library + Neconsys Client Package
- GZ: C++, MFC Library + Neconsys Local Package
➣ Source原本保管场所及权限
- Neconsys保管所有原本Source
➣ Package管理方法
- Neconsys开发后打patch


**==OA Client无法打开==**
➣ 防火墙都已开通，都能telnet进端口
- 1234/2345/8676/21/3433
➣ 输入账号密码后，等很长时间才出选择工种页面，选择任一工种后提示无ncs文件
1. **FTP模式改为被动模式**


**==现场UMS软件运行后，几秒后自动关闭==**
➣ 重启电脑也一样
1. 查看UMS log，定位问题
2. log显示opc连接异常
3. 确认KEPServerEX得知，有人把opf文件设置成了simdemo.opf
- simdemo.opf是demo，不是正式
4. 导入正确的opf文件，重启UMS软件后恢复正常
- opf设置错误，导致PLC无法与UMS正常通讯，PLC一直尝试通讯，导致UMS 软件负荷变高，所以软件会自动关闭


**==设备定时自动启停相关==**
- 空调班用的比较多
- 不是所有工种的设备都可以用，PLC对应的驱动(UMS)需要支持该功能。如不支持，需要重新开发驱动


**==无法在UMS上控制恒温恒湿机启动停止==**
➣ 现场启动停止恒温恒湿机，UMS上状态会变化
1. **确认设备模式是 本地(local)还是远程(remote)，只有远程模式下才能在UMS上控制**


**==Local Client没有License症状==**
➣ 运行软件提示，you must have program registration
➣ 所有实时数据为??，无法点击任何管制点
★ **License登录方法(A)**
1. **关掉Local Client软件**
2. **移动到软件安装目录，以管理员权限运行cms.exe**
3. **语言转为韩国语**
4. **도움말 - TmsView 정보(A)** 
5. **按住Ctrl键，双击左边的图标两下**
6. **下面的Input Key下面填写License密码**
- **==密码：94o6o1==**
- o是英文小写
7. 关掉cms.exe，重新打开软件确认

★ **License登录方法(B-正规)**
1. **复制粘贴license文件到Local** PC
- license备份路径: FTP→chengyingxu→backup→License下的ini文件
- Local PC路径: C盘→Windows文件夹
- ini文件名只保留ncmacsk，后面的都去掉
 2. **重新启动ums软件确认**


**==大气班本地监控账号密码==**
- a / a


**==UMS IF服务器相关==**
- UMS IF → E-FDC
- 基准信息: 每天13点 / 21点
- raw data : 每分钟(analog, S/A等级，连续形)


**==KEPServerEX破解方法==**
![[20250529120801-1748491681373-v2-9081d362915ef707561a5f4ba1b51e01_r.jpg]]
![[20250529120813-1748491693930-v2-fede85460d26a75622d8079820336213_1440w.jpg]]
![[20250529120819-1748491699800-v2-fc4cd4903dc661ac1041180364c4f101_1440w.jpg]]
![[20250529120824-1748491704613-v2-a9870198fbd0f7d0a91e20a13fce94cd_r.jpg]]
![[20250529120827-1748491707467-v2-24e73c6fde9f7f3e40445fbdee112e63_1440w.jpg]]
![[20250529120835-1748491715823-v2-d089638605b70660fa00f00430498225_1440w.jpg]]
![[20250529120837-1748491717942-v2-840b5613255a9ebbef014510913cf318_1440w.jpg]]
- 软件都要用管理员权限运行


**==模拟量数据变换==**
![[IMG_20250606_092720.jpg]]
- 변환최소값 / 변환최대값 : 가공범위
- 설비최소값 / 설비최대값 : 미가공범위

- PLC值:1000，加工范围:0-16000
   未加工范围:0-10
- Tag值=0+(1000-0)×(10-0)/(16000-0)=0.625


**==运行DMS/FMS Local client报错，无法运行==**
➣ 报错信息：Failed to Load Device Dll:..... DevDes.dll
- 只有DMS/FMS才报这个错
- Device文件夹里的删除DevDes.dll或者在ini配置文件里注释掉DevDes.dll时，能正常打开
★ 原因: 没有安装TIB_rv软件导致，按照教程安装即可
- DMS/FMS通过TIB_rv软件发送数据给DES


**==TIB_rv安装相关==**
➣ 软件/安装教程License文件
- UMS FTP→Necon back up→P10A DMS PC Setup   
- 安装教程里3-3License适用里的参数有可能输不进去    
- services.msc里启动TIB服务，启动设置为自动启动
➣ 服务确认
- 网页输入 [http://localhost:7580](http://localhost:7580)


**==大气班Sub PC的ini设置==**
- SystemSave=0
- SystemQuery=0
- ServerSave=0
- ServerQuery=1
- RTPSSend=0
- EssSend=0
- Upload=
- Download=1


**==Local Client取消登录账号的ini设置==**
1. **更改cms.ini配置文件**
- LogIn=0


**==postgreDB设置时报错==**
➣ 运行CreateDB_Database.sql时，报错文件夹permission is denied
1. **给CmsLocalDB文件夹权限，User和Administrator都给全部权限**


**==运行OA Client时报错==**
➣ 错误代码如下:
![[1753767598234.jpg]]
➣ 特定工种(如HVAC)报错，其他工种正常
➣ 重新安装OA Client也一样
➣ 该工种的Local Client正常
★ 打开PT.ncs文件保存，进入到RAWPOINT_TABLE时发现出现数据格式错误
1. **在Local Client里点击upload**
2. **重新运行OA Client**


**==关于Local Client通过FTP上传配置文件==**
1. **各Local Client每天凌晨会下载FTP服务器上的cfg文件**
- 每个工种有一个cfg文件，cfg文件里包含该工种的所有config文件信息。
2. **比较Local Client本地保存的config文件的大小与cfg上的config文件的大小并操作**
- 大小一致→Local Client不上传config文件
- 大小不一致→Local Client上传有变化的config文件


**==FTP服务器上cfg文件的构成==**
- 文件名 | 文件大小 | 更新时间
- 文件大小的单位为Byte


**==CO UMS Local PC IP==**
- BOILER_CHILLER : 10.72.136.229
- DI : 10.72.136.233
- DI(私网1) : 192.168.0.233
- DI(私网2) : 192.168.0.234
- DMS : 10.72.136.199
- FMS : 10.72.136.91
- G2 Pub : 10.72.137.112
- G2 Non-Pub : 10.25.69.19 / 192.168.0.202
- HVAC : 10.72.136.226
- HVAC(sub) : 10.72.136.228
- IW : 10.72.136.234
- N2CDA : 10.72.136.231
- REUSE : 10.72.136.236
- SCADA : 10.72.136.238
- SCR_VOC : 10.72.136.232
- SCR_VOC(sub) : 10.72.136.230
- SPC : 10.72.136.195
- WWT : 10.72.136.235


**==关于FTP==**
★ 网页登录: 被动模式
★ OA Client的模式设置
➣ 被动模式
- ums_ftp01 : SmartClient.ini配置文件→Update行中将密码(!qaz2wsx)后的0变更为1
- ums_adm01 : Cms.ibi配置文件→[FTP-CONFIG]下将Mode改为1
➣ 主动模式
- 与上述相反，变更为0


**==KR Cloud登录CO OA Client异常==**
➣ CO Cloud登录CO OA Client正常
➣ UMS服务器的ftp Log里报错 LIST 550
- Log路径: C:\inetpub\Logfiles\FTPSVC2
1. **将连接方式变更为被动模式**
- 默认设置为主动模式，具体设置方法参考上面


**==MELSEC PLC无法连接UMS系统==**
➣ 能ping通，但是无法连接UMS系统
1. **进入MELSEC PLC的IO设置界面**
2. **点击无法连接UMS系统的PLC**
- 例: 06: 1-10.72.137.105, 2-10.72.137.106
3. **下方PLC信息中找到端口编号，默认是1500，改为1505**
- MELSEC PLC如出现通讯异常会dro掉默认端口，1505是1500的下一个端口
4. **点击中间的修改按钮**
5. **提示设备会停止通讯，点击确定**



**==SCADA Local PC出现异常多的报警==**
➣ Fam PC只有几百个报警，但SCADA Local PC的历史报警里有几千个报警，且多为未解除的报警
1. **确认SCADA Local PC的Log，确认出现大量异常报警的时间段有无重启UMS软件的记录**
- 已报警但未解除的报警，当重启UMS软件时，UMS会将未解除的报警重新弹窗并记录在历史报警里，但重新记录的报警的时间会记录为当前报警时间，这样就导致同一个报警重复记录
2. **在cms.ini文件里添加以下配置**
- [SYSTEM-ALARM]
- StartAlarmQry=1
3. **重新启动UMS软件**


**==WWT Local Client一分钟内闪退==**
➣ log里有大量opc通讯异常log
★ KEPServerEX软件未启动，出现大量opc连接请求，导致软件崩溃
1. **运行KEPServerEX软件**
2. **打开opc配置文件**
- 2000901_WWT_OPC_DH1811_DH1812.opf
3. **运行UMS软件**

