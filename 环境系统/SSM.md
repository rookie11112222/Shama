**==Server连接(AP/IO) - CO3.0==**
★ **root权限获取**
1. **使用cgssadm账号登录**
2. **sudo su - cgssadm**
3. **sudo su -**
➣ AP服务器
- SSM: cgssadm / pjSSMsvc0!
- WAS: jussvc01 / pjSSMsvc0!
- WEB: websvc01 / pjSSMsvc0!
➣ JEUS网页
- http://10.72.134.46:9736/webadmin/login
- administrator / jeusadmin
➣ IO服务器
- GAS: cgssadm / pjSSMsvc0!
- Chemical: ccssadm / pjSSMsvc0!
- ESS I/F: cgssadm / pjSSMsvc0!
➣ NoSql服务器
- cgssadm / pjSSMsvc0!
➣ Cache服务器
- cgssadm / pjSSMsvc0!
➣ DB服务器
- cgssadm / pjSSMsvc0!
➣ Local IO
- administrator / 1Q2w3e4r5t!
➣ MSSQL临时
- sys_adm01 / root123#


**==CO开发服务器==**
- SSM_DEV / 10.72.134.31 / LGDCSSC001D
- sys_adm01 / smJULY2@


**==DB连接==**
- spg_app / Sgp_app!0405 (3.0)
- sgp_mgr / Sgp_mgr!0405 (3.0)


**==NoSql_3.0==**
- sgpapp / sgpapp2014 (사용자)
- sgs_admin / sgs_admin01 (관리자)


**==SSM网页==**
- sgp_admin01 / spino_admin01(3.0)


**==Smart Builder==**
- CO(3.0) : http://10.72.134.48:8088
- 账号密码和SGP网页的一样
- Online模式编辑并发布后，一定要转成Offline模式保存在本地上


CGSS Local IO PC
➣ 2.0
- Administrator / !q2w3e4r5t
- radmin: SSM / 1q2w3e4r5t (Radmin)

➣ 3.0
- Administrator / 1Q2w3e4r5t!


**==防火墙端口==**
- 843/3210/8081/8088/9888/9999/10010/10011/10012/10013/10014/10015/10021/10083/10093/10113/10141/10143/20201


**==Smart Connect==**
- CGSS Local IO #1(137.94)进去，或者2.0的AP服务器也可进去
- http://10.72.134.49:9288/smartconnect  (Gas Supply IO)
- http://10.72.134.52:29288/smartconnect  (Chemical Supply IO)
- http://10.72.134.55:9288/smartconnect  (ESS I/F IO)
- http://10.72.137.94:9288/smartconnect   (Gas Leak IO)
- admin  /  admin


**==应急管理局(10.72.78.22)==**
- admin  /  2020sDbn#0917M


**==应急管理局(10.72.78.22:1026)==**
- towery  /  T0wery@!@#Ha2019
- Chrome打开


==**vertex登录信息**==
- administrator / vertex


**==名词解释==**
- CGSS: Central Gas Supply System
- CCSS: Central Chemical Supply System
- GL: Gas Leak
- WL: Water Leak
- Safety: Water Leak, MOV, 紧急排气，CO2 Package, Gas Leak, Chemical Leak
- MOV: Motor Operated Valve 电动阀
- VMB: Valve Mainfold Box 阀门分配箱


**==CCSS==**
![[IMG_20230227_162205.jpg]]
- ACQC(Auto Clean Quick Coupler) : Tank에 Chemical을 유입하는 설비
- Tank : Chemical 보관
- Buffer : Tank와 Supply중간에 Chemical 공급하는 설비
- Supply : 생산설비에 Chemical 공급하는 설비
- Mixing : Chemical 혼합하는 설비
- 위험물 저장소 : 화재 및 폭발 위험성이 있는 Chemical류를 보관하기 위한 저장소(공장 외부에 설치)
- 유독물 저장소 : 위험물 저장소에서 온 Chemical 및 유독물 Chemical을 보관하는 저장소
- Solvent(위험물) : 용매라 하며 용질을 녹여 용액을 만드는 물질
- Etchant(유독물) : 금속 또는 비금속표면을 화학적으로 부식하는 물질


**==Local IO(GMS)添加路由方法==**
```
route -p add 192.175.1.0 mask 255.255.255.0 192.175.1.1
```
- 192.175.1.0 : 192.175.1.x网段


**==删除已经添加的路由方法==**
```
route delete 192.175.1.0
```
- 192.175.1.0: 192.175.1.x网段


**==显示已经添加的路由==**
- route print -4


**==AP服务器 Java dump生成方法==**
```
jmap -dump:format=b,file=路径/文件名称.hprof <PID>
```
- 例如：jmap -dump:format=b,file=/ENGN/test.hprof 10000
- 上述命令行在JEUS部署的服务器命令行中运行，AP服务器
- PID在Task Manager上确认(jeus服务的PID)


**==异常感知信息==**
- 想使用异常告知的发信息功能的话，生成账号时要把使用者设成 CMS，不然会报错


**==确认Cassandra Compaction的方法==**
1. **移动到cassandra的bin目录**
```
cd /ENGN/cgssadm/sw/datastax-ddc-3.9.0/bin
```
2. **确认当前进行中的compaction作业**
```
./nodetool -p7199 compactionstats
```
3. **确认当前compaction的默认数量**
```
./nodetool -p7199 getcompactionthreshold sgs th_sha_hist
```
4. **变更compaction默认数量**
```
./nodetool -p7199 setcompactionthreshold sgs th_sha_hist 4 8
```
- 默认值: 4为最小，8为最大


**==FCR문자==**
- F:Fault  C:Cause  R:Resolution


**==CO2Package SSM重置后颜色不变绿==**
1. **打开SmartBuilder**
2. **进入CO2 Package Alarm Reset页面，确认重置按钮Mapping的Script**
3. **进入CO2 Package主页面，找到需要重置的设备，点击该设备名字和背景画面，确认里面的Tag内容**
4. **对比2和3确认的Tag是否一致，2和3需要一致才行**


**==异常感知_文字发送履历里无法打开或下载excel文件==**
➣ 原因: 发送的文字里有系统无法识别的特殊符号
★ 解决方法:DB里替换掉文字内容(去除特殊符号后)
1. 连接进DB
2. 用update语句替换文字内容
```
update [SGP].[TN_CMS_ABNOR_MMS_H]
set MESSAGE_TEXT = '替换后的内容'
where MMS_SEND_ID = '异常短信的编号'
```
- [TN_CMS_ABNOR_MMS_H]
- [TN_CMS_ABNOR_MMS_SEND_H]
- 按这种方法，两个表都要操作


**==不使用的异常感知SMS群删除==**
➣ 网页上只能设定[不使用]，需要在DB里删除
1. **网页上去到需要删除的SMS群，删除里面的所有人员**
2. **连接DB后运行删除sql**
```
delete from [SGP].[SGP].[TN_CMS_ABNOR_RECEIVER_GROUP_N] where RECEIVER_GROUPNAME = 'test'
```
- 两个DB都要操作


**==人事IF过来的Table==**
- TN_SSM_HR_M


**==이벤트 이력里出现多条相同的Event==**
- MMI权限里赋予的设施物太多，有些设施物的Event接收勾选重复导致
- 重新确认设施物Event接收勾选


**==异常感知信息/Leak自动信息发送==**
- CO：SSM→SMS服务器→M-Talk/kakaotalk/韩国一般信息
- 必须安装手机版M-Talk才能收到信息，且手机端M-Talk正常登录


**==Leak自动信息相关==**
- HMI共同
- 必须要在网页添加人员和收信群组，不可以使用DB插入
- DB插入的数据无法同步到内存，需要重启服务才可以


**==SSM3.0 SmartViewer里点击事件履历闪退==**
- IE/Edge浏览器出问题
- IE：Internet选项→隐私→[启用弹出窗口阻止程序] 去除勾选
- Edge：设置里搜索[弹出窗口阻止]


**==Linux测试某个端口是否通==**
- nc -zvw 1 [IP地址] [端口号]


**==SmartBuilder 3.0 画面发布方法==**
1. **更改画面保存后，关掉所有打开的画面**
2. **打开需要发布的画面**
3. **유틸리티>프로젝트 원격 배포** 
4. **[선택 화면 파일] 선택 후 오른쪽의 [배포리스트]에 배포할 화면이 나오는지 확인** 
5. **[화면 태그 목록 저장] 체크하고 왼쪽 하단의 [전체 선택] 체크하고 [배포] 버튼을 누른다**
6. **정상 배포 완료되면 메시지창에 [화면 태그 정보 저장이 완료되었습니다.]가 나옴**
7. **뷰어 열어서 정상 배포됬는지 확인한다**


**==IE浏览器设置桌面快捷方式==**
1. **找到IE浏览器的安装路径**
- 开始→Windows附件→鼠标右键点击IE图标→更多→打开文件位置
2. **桌面会生成IE图标，右键属性，上面快捷方式，目标栏拉到最后，空格后填写SSM网址**
- http://10.72.134.48:3210


**==CO SSM 3.0 网页==**
- http://ssmco1.lgdisplay.com:3210/sso/login/loginWithId.do
- 上面网页是lets上登录的
- 和SSO联动


**==ID정보가 확인되지 않습니다==**
- 没有SSM账号
- 有SSM账号，但设置为未使用


**==Gas Leak的Event发生时间正常，查看Trend时保存时间与正常时间相差15个小时==**
 ➣ CGSS Local IO PC的时区设置为US时区的状态下，运行SmartConnect导致
★ 更改为正确的时区，重启SmartConnect服务
- Event保存时间按照AP服务器，Trend保存按照Local IO PC时间


**==画面上显示和PLC通讯异常==**
- 所有点位显示灰色斜杠
- PLC 能Ping通，端口可以Telnet
- SC上该Device上的Tag的值全都是null，需要点READ才能读取值
1. **全选该Device的Tag，点击SET COV**
- 在SC网页上操作


**==Application 启动确认(CO 3.0)==**
- 以下均为TCP协议
- Linux：netstat -anp | grep 端口
- Windows：netstat -ano | findstr 端
➣ AP
- 3210 - WebtoB (HTTP Server)
- 8088 - JEUS
- 9777 - SGP Push
- 8070 - Critical Channel
- 8071 - Normal Channel
- 9999 - Push.PolicyServer
- 9888 - Push.RelayServer
- 11001 - external cov
- 11002 - external event
- 10010 - common
- 10011 - alarm
- 10012 - logty
- 10013 - viewer init
- 10014 - alarm web
- 10015 - common web
- 10030 - 其他
- 10031 - 其他
➣ DB
- 3521 - DBMS(Oracle DB)
➣ IO
- 9090 / 9288 / 1090 - Gas 服务
- 29090 / 29288 / 21090 - Chemical服务
➣ NoSql
- 9042 - Cassandra服务
- 7199 - NodeTool
➣ Cache #1
- 7001 - Common Master
- 7021 - Alarm Master
- 7061 - Event Master
- 7103 - Common Slave
- 7043 - Alarm Slave
- 7083 - Event Slave
➣ Cache #2
- 7002 - Common Master
- 7022 - Alarm Master
- 7062 - Event Master
- 7101 - Common Slave
- 7041 - Alarm Slave
- 7081 - Event Slave
➣ Cache #3
- 7003 - Common Master
- 7023 - Alarm Master
- 7063 - Event Master
- 7102 - Common Slave
- 7042 - Alarm Slave
- 7082 - Event Slave


**==Data保存期限==**
- Trend : 1年
- Event : 没有限制
- 异常感知信息发送履历 : 不限制


**==Water Leak관련==**
- Break: 센서 꾸그려졌거나 일부 단선 발생시 생기는 것, NW통신과는 별개
- Break 상태에서 Leak 감지되면 Leak 팝업뜨고 이벤트 이력으로 저장됨
- HMI의 색변화는 빌더의 색변화의 우선순위를 따른다. 즉 Break가 우선순위이면 Break 상태에서 Leak가 감지되면 이벤트 팝업은 뜨나 HMI상은 Leak 색상으로 유지. 
- 발생한 이벤트중 복구가 안된 이벤트는. WAS 재기동할때 다시 띄워준다.
![[eccdc1059e69496bae8cac81f50e6a23.jpg]]


**==SSM3.0(CO) SGP Enterprise服务启动顺序==**
1. **NoSql (Master→Slave)**
- D8AGCCP01→D8AGCCP02
2. **Cache (Master→Slave)**
- Master: D8AGCCP01→D8AGCCP02→DD8AGD8AGCC03
- Slave: D8AGCCP02→D8AGCCP03→D8AGCCP01
3. **Push Server**
- D8AGCAP01→D8AGCAP02
4. **WebtoB** 
- D8AGCAP01→D8AGCAP02
5. **JEUS**
- D8AGCAP01→D8AGCAP02
6. **Gas IO**
- D8AGIOP01→D8AGIOP02
7. **Chemical IO**
- D8ACIOP01→D8ACIOP02
8. **ESS IO**
- D8AGCEP01→D8AGCEP02


**==SSM3.0 HMI上的数据与IO服务器的关系==**
1. **IO服务器收集现场设备的数据**
2. **当数据与之前数据不一致时，IO服务器将新的数据传送给AP服务器**
3. **AP服务器将接收到的数据先放到Cache，然后再传输给Client**


**==Local IO PC故障处理==**
★ **方法1**
1. **提前备份好active pc的数据**
2. **将数据移动到standby pc**
3. **将standby pc的IP变更为原来active pc的IP**
- 需要对IP进行解绑后重新绑定
4. **standby pc启动SC**

★ **方法2**
1. **SmartBuilder上将local io pc的IP变更为standby pc的IP**
2. **standby pc启动SC**
3. **standby pc上登录SmartConnect 网页后设置AP服务器的IP和端口**
- ENTERPRISE页面下的channel_high和channel_normal的Address
- channel_high : http://10.72.134.48:8070/enterprise/sci/
- channel_normal : http://10.72.134.48:8071/enterprise/sci/


**==AP服务器和IO服务器双重化相关==**
- AP: Active-Active构造，哪个挂了都不影响服务，无缝衔接
- IO: Master-Slave构造，Slave挂了不影响服务，Master挂了takeover时间影响服务(数秒到1分钟)


**==System 构成相关==**
1. **构成状态**
- CO: Cityhub Building 3.0 + CO定制开发
- CA: Cityhub Building 2.0 + CA定制开发
2. **Source原本保管场所及权限**
- 坡州 LGD SVN服务器 / 无权限
3. **Package管理方法**
- solution研究所(CNS)管理，需要变更时，整合坡州/龟尾/CO的所有需求后进行patch


**==patch相关==**
- html : /ENGN/cgssadm/SGP/sgp_pkg/sgpDomain/WEB-INF/templates
- class : /ENGN/cgssadm/SGP/sgp_pkg/sgpDomain/WEB-INF/classes
- xml : /ENGN/cgssadm/SGP/sgp_pkg/sgpDomain/WEB-INF/classes/META-INF/sqlmap_oracle
- html/js/css文件改名后立即适用，class/xml文件需要重启WAS服务后适用
- 需要两台AP都要更改


**==SSM3.0 cgssadm无法正常启动SC==**
➣ 用cgssadm只能启动Agent，无法启动Engine
➣ 用root账号Agent/Engine都能启动
1. **切换到root账号**
2. **移动到/ENGN/cgssadm/SGP/SmartConnect/Engine/configuration/**
3. **删除org.eclipse.osgi整个文件夹**
4. **切换到cgssadm**
5. **重新启动SC**
- 不可以用root启动停止SC，必须要用cgssadm，如不小心使用了root启动SC，需要先切到cgssadm后停止SC，再启动SC


**==登录进网页后报错==**
➣ 可以打开网页，输入账号密码登录后报错，错误代码:403
★ 该账号设置的初始登录页面没有访问权限
1. **登录进admin账号**
2. **右上角 basic**
3. **点击旁边的齿轮图标**
4. **右边 시스템관리→ 사용자，搜索有问题的账号后，编辑初始登录页面(设置为有权限的页面)**


**==web권한 확인 방법==**
1. **SSM → web권한관리 → 권한그룹별 사용자 관리**
2. **본인 계정이 가지고 있는 권한을 검색한다**
3. **검색결과에서 Menu Permissions으로 이동해서 설정**


**==HMI화면에서 이벤트 필터링 수신 관련==**
➣ Gas Leak 이벤트중 Leak만 수신하고 Fault는 수신 안함
1. **빌더 로그인한다**
2. **그룹관리 → 태그그룹관리** 
3. **GAS_LEAK폴더에 10_Fault라는 하위 폴더 생성한다**
4. **이름은 원하는데로 설정한다**
5. **유형은 일반, 속성 유형은 설비로 설정한다** 
6. **GAS_LEAK의 다른 폴더로 이동하여 Fault를 매핑해제한다**
7. **폴더 클릭 → 중간 검색창에 Fault 검색 → 전체 선택하고 오른쪽상단의 해제 누른다** 
8. **10_Fault 폴더로 이동하고 오른쪽상단의 매핑 버튼 누른다** 
9. **나오는 팝업창에서 Fault로 검색하고 선택한다**
- 한개 태그는 한개 설비(폴더)에 매핑하는걸로 권장한다 
- 이벤트는 매핑한 설비만 수신할수 있다 


**==web조회조건↔HMI 이벤트 수신↔이벤트 이력 상관관계==**
★ **web조회조건 조회 방법**
1. **관리자 → web권한 관리 → 권한그룹별 사용자 관리**
2. **본인이 소속된 권한그룹을 찾는다** 
3. **권한명을 클릭하여 Name을 확인한다**
- ROLE_로 시작한다
- 여러개 권한이 있으면 다 확인한다
4. **관리자 → web권한 관리 → web조회조건 권한 관리**
5. **3에서 확인한 권한명을 검색하고 클릭한다**
6. **나오는 화면에서 이벤트 수신할 항목을 선택한다**

★ **상관관계**
- 빌더에서 권한그룹에 설비를 매핑해줘야 HMI화면에서 해당 설비의 이벤트 수신 가능
- web조회조건의 설정을 통해 HMI화면의 이벤트 수신과 이벤트 이력 조회를 필터링 가능하다

★ **한개 계정에 여러개 객체권한이 있을 경우**
- A권한 : Gas Leak, 1등급
- B권한 : MOV, 2/3등급 
- 빌더에서 A/B권한에 각각 모든 Gas Leak설비와 MOV설비 매핑
➣ **==HMI화면과 이벤트 이력 조회시 Gas Leak와 MOV의 1/2/3등급 모두 수신 및 조회 가능==** 
- web조회조건은 or 조건이다 
- 그래서 원칙적으로는 한개 계정에 한개 객체권한을 매핑해야 한다


**==SC网页无法打开==**
➣ SC网页无法打开，防火墙无阻拦，loading很久后最终无法打打开
➣ SC Agent的catalina报错OOM
- /ENGN/ccssadm/SGP/SmartConnect/Agent/apache-tomcat-8.5.64/logs
1. **单独重启Agent服务**
- Agent不影响数据接收等服务
- Engine : 接收数据
- Agent : SC网页


**==SSM服务启动时的相关注意事项==**
➣ 启动WAS服务时，时间会比较久
➣ 启动IO服务后，需要确认与AP服务器能否正常通讯
- IO的SC网页→ENTERPRISE里的两项要为running状态
- 在IO服务器上使用nc命令连接AP服务器的服务IP，要联通
- 如上述都为NG，需确认L4交换机
➣ 启动tib服务后，如设备还未发送信息，SC网页会显示stopped，收到信息后会转变未running状态
➣ 重启Gas Leak的服务后，SC网页上vertex有可能显示为stopped，需要手动一个一个远程连接进去
➣ 启动AP服务器上的服务时，启动一个服务后等待1-2分钟后，再启动下一个
- push→web→was
- 启动服务时，最好打开AP服务器上的error.log文件，实时查看有无报错(tail -f)


**==tib服务监听命令==**
1. **移动到tib安装目录下的bin目录**
2. **运行下列命令**
```
./tibrvlisten -service "30001" -network ";239.30.0.01;" -daemon "tcp:30021" "TEST"
```
- service等信息可在Chemical IO的SC网页上确认到


**==更改IO服务的Master和Slave顺序==**
➣ 现在是#1为Slave，#2为Master，想要变为#1为Master，#2为Slave
1. **登录进#2的SC网页→SYSTEM MANAGEMENT**
2. **点击Action项里的중지，稍等一会**
3. **登录进#1的SC网页后，确认是否变为Master**
- SC首页显示SCD, TAG等数量为Master，都显示为N/A则为Slave
4. **登录进#2的SC网页→SYSTEM MANAGEMENT**
5. **点击Action里的시작，稍等一下**
- 虽然网页显示为stopped，但其实是运行状态


**==Oracle DB邮件发送==**
1. **打开dbeaver软件**
2. **输入以下调用语句发送测试邮件**
```
DECLARE p_to VARCHAR2(50):= "chengyingxu@lgdpartner.com"
p_from VARCHAR2(50):= "chengyingxu@lgcns.com"
p_subject VARCHAR2(50):= "test"
p_text_msg VARCHAR2(50):= "test123123"
p_html_msg CLOB

BEGIN sp_send_mail (
	p_to,
	p_from,
	p_subject,
	p_text_msg,
	p_html_msg
);
END;
```
- p_to: 收信人
- p_from: 发信人，可为空
- p_subject: 邮件标题
- p_text_msg: 邮件正文内容
- VARCHAR2(50): VARCHAR2数据类型不能变，括号内的可以根据字符长度变更