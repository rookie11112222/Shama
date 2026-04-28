**==where语句无法查找中文/韩文字段==**
➣ 字段为nvarchar
```
select * from［Table Name] 
where [字段名] = N'小明'
```
➣ 字段为varchar
```
select * from［Table Name] 
where [字段名] = '小明'
```


**==截取字符串中的某一段==**
```
select * from [Table Name] with(nolock) 
where substring(要截取的字段名，截取开始位数，截取长度) = ‘a’
```


**==查找包含某个关键字的结果==**
```
select * from [表名]  
where [colunm名] like '%关键字%'
```
- %为通配符


**==join语句使用==**
```
select * from [A表] M 
join [B表] R
on M.RCVER_MANAGE_NO = R.RCVER_MANAGE_NO 
order by M.CREATION_TIMESTAMP desc
```
- A表和B表Join，匹配RCVER_MANAGE_NO，降序排序


**==sqlserver查看并修改数据库文件块设置==**
1. **连进数据库**
2. **左边 Databases→NeconServer(数据库名)**
3. **NeconServer(数据库名)右键→Properties→Files**
- Initial Size(MB): 分配的初始总空间
- Autogrowth/Maxsize: 自动增长及最大设置


 ==**sqlserver确认各数据库文件块大小及使用量**==
1. **连进数据库**
2. **左边 Databases→NeconServer(数据库名)**
3. **NeconServer(数据库名)右键→Reports→Standard Reports→Disk Usage**
- 出现页面中间部分，Disk Space Used by Data Files中可以看到使用量


**==sqlserver确认数据文件大小==**
```
select * 
from [数据库名].dbo.sysfiles
```
- 数据大小单位为bit
```
select name, convert(float，size)*8/1024/1024 as size  
from [数据库名].dbo.sysfiles
```
- 数据大小转换为GB


**==sqlserver查看数据库分配的空间和剩余空间==**
```
exec sp_spaceused
```


**==sqlserver确定数据库数据文件详细使用情况==**
```
dbcc showfilestats
```
- 单位为extend，1extend = 64KB


**==sqlserver确认log文件详细使用情况==**
```
dbcc sqlperf(logspace)
```


**==查询字段长度大于5的结果==**
```
SELECT * FROM [表名]  
WHERE LEN(字段名) > 5
```
- 字段需为字符串


**==显示所有相关表==**
```
SP_HELP
```


**==sqlserver批量删除表==**
1. **提取drop table脚本** 
```
SELECT 'drop table' + name + ' ' + ';'  
FROM sys.tables  
WHERE NAME LIKE 'A%'
```
- A为Alarm(UMS)表名开头，H为历史数据(UMS)表名开头
2. **复制第一步的运行结果后，在SQL上执行**
- 将第一步执行出的结果复制粘贴到New Query后运行


**==数据库收缩==**
1. **恢复模式变更为Simple**
```
ALTER DATABASE NeconServer
SET RECOVERY SIMPLE;
```
- NeconServer为数据库名
2. **压缩数据库**
```
DBCC SHRINKDATABASE(NeconServer)
```
3. **恢复模式变更为Full**
```
ALTER DATABASE NeconServer
SET RECOVERY FULL;
```
- 数据库收缩时，需要一定的硬盘容量，压缩200G，大概需要8G的空间
- 先删表，后收缩


**==确认硬盘空间的物理使用量==**
```
EXEC xp_fixeddrives
```


**==确认数据库数据的硬盘使用情况==**
```
select ISNULL(DF.DriveLetter, LF.DriveLetter) DriveLetter, DataSize_GB, LogSize_GB
from 
(
select left(physical_name,1) DriveLetter, CAST(sum(size)/128.0/1024 AS DECIMAL(10,2)) DataSize_GB
from sys.master_files
where type_desc = 'ROWS'
group by left(physical_name,1), type_desc
) DF
full outer join
(
select left(physical_name,1) DriveLetter, CAST(sum(size)/128.0/1024 AS DECIMAL(10,2)) LogSize_GB
from sys.master_files
where type_desc = 'LOG'
group by left(physical_name,1), type_desc
) LF
on DF.DriveLetter = LF.DriveLetter
```


**==确认数据库相关设置==**
```
SELECT NAME AS [DB명],
CASE WHEN IS_AUTO_CLOSE_ON = 1
THEN 'Y' ELSE 'N' END AS '자동 닫기',
CASE WHEN IS_AUTO_SHRINK_ON = 1
THEN 'Y' ELSE 'N' END AS '자동 축소',
CASE WHEN IS_AUTO_CREATE_STATS_ON = 1 
THEN 'Y' ELSE 'N' END AS '통계 자동 작성',
CASE WHEN IS_AUTO_UPDATE_STATS_ON = 1
THEN 'Y' ELSE 'N' END AS '통계 자동 업데이트',
CASE WHEN IS_AUTO_UPDATE_STATS_ASYNC_ON = 1
THEN 'Y' ELSE 'N' END AS '통계를 비동기적으로 자동 업데이트',
STATE_DESC AS [상태],
RECOVERY_MODEL_DESC AS [복구모델],
FROM SYS.DATABASES
```


**==确认数据库文件信息==**
```
EXEC SP_HELPDB NeconServer
```
- NeconServer为数据库名


**==确认数据库文件的使用量==**
```
use NeconServer
select      SERVERPROPERTY('ComputerNamePhysicalNetBIOS') as HOSTNAME,
ISNULL(SERVERPROPERTY('InstanceName'), 'MSSQLSERVER') as INSTNAME,
(select db_name()) as DBNAME,
CONVERT(varchar(10),getdate(),126) as Date,
sysfiles.name as LogicalFileName,
CAST(sysfiles.size/128.0 as int)  as 'FileSize(MB)',
CAST(CAST(FILEPROPERTY(sysfiles.name, 'SpaceUsed') as int/128.0 as int) as UsedSpaceMB,
CAST(sysfiles.size/128.0-CAST(FILEPROPERTY(sysfiles.name, 'SpaceUsed') as int)/128.0 as int) as FreeSpaceMB,
CAST(100-100*(CAST(((sysfiles.size/128.0-CAST(FILEPROPERTY(sysfiles.name, 'SpaceUsed') as int)/128.0)/(sysfiles.size/128.0)) as DECIMAL(4,2))) as varchar(8)) as UsedSpacePct,
sysfiles.filename as PhysicalFileName
from
dbo.sysfiles
left outer join sysfilegroups 
on sysfiles.groupid = sysfilegroups.groupid
order by 
sysfilegroups.groupid, filename
```


**==限制返回结果的行数==**
```
SELECT TOP 3 column1
FROM table_name
```
- 返回前3行的数据
```
SELECT TOP 10 PERCENT ccolumn1 
FROM table_name
```
- 返回前10%的数据


**==复制表和表结构==**
```
SELECT * INTO 新表 FROM 旧表
```


**==SID确认方法==**
```
SELECT @@version as Version, SERVERPROPERTY('ServerName') AS ServerName,  SERVERPROPERTY('MachineName') AS MachineName,  SERVERPROPERTY('InstanceName') AS InstanceName,  SERVERPROPERTY('Edition') AS Edition
```
- InstanceName = SID
- MachineName = HostName


**==schema确认方法==**
```
exec sp_helpdb
```
- Name = schema


**==确认procedure运行的相关信息==**
```
select top 100 db_name(d.database_id) as DBName,
s.name as 存储名称,
s.type_desc as 存储类型,
d.cached_time as SP添加到缓存的时间,
d.last_execution_time as 上次执行SP的时间,
d.last_elapsed_time as 上次执行SP所用的时间(ms),
d.total_elapsed_time as 完成此SP的执行所用的总时间(ms),
d.total_elapsed_time / d.execution_count as 平均执行时间(ms),
d.execution_count as 自上次编译以来所执行的次数
from sys.procedures s
join sys.dm_exec_procedure_stats d
on s.object_id = d.object_id;
```


**==DB变成suspect状态==**
➣ 确保DISK等HW无问题
➣ 最好用sa账号登录
➣ NeconServer为DB名
1. **重置数据库状态**
```
EXEC sp_resetstatus 'NeconServer'
```
2. **设数据库为紧急模式**
```
ALTER DATABASE NeconServer 
SET EMERGENCY 
```
3. **检查数据库**
```
DBCC CHECKDB('NeconServer')
```
4. **设置数据库为单用户模式**
```
ALTER DATABASE NeconServer 
SET SINGLE_USER WITH ROLLBACK IMMEDIATE
```
5. **修复数据库**
```
DBCC CHECKDB('NeconServer', REPAIR_ALLOW_DATA_LOSS)
```
6. **设置数据库为上线模式**
```
ALTER DATABASE NeconServer 
SET ONLINE
```
7. **设置数据库为多用户模式**
```
ALTER DATABASE NeconServer 
SET MULTI_USER
```




