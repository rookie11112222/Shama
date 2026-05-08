**==时间戳(timestamp)字段转换为字符串==**
- TO_CHAR(字段, 'YYYY-MM-DD HH24:MI:SS')


**==截取字符串==**
- SUBSTR(字段, 起始字符, 截取长度)


**==空字段处理函数==**
- nvl(表达式，结果)
- nvl(员工工资,0) --  员工工资为空，返回0; 员工工资不为空，返回员工工资


**==空字段排序==**
```
select * from [表名] 
order by [字段] desc nulls first
```
  - 降序排序，空值放前面
```
select * from [表名] 
order by [字段] desc nulls last
```
  - 降组排序，空值放后面


==**字符串拼接**==
➣ || (拼接符)
```
select [姓名] || 'a' from [表名]
```
  - 姓名字段后拼接a字符，也可以字段和字段拼接
➣ concat函数
```
select concat(字段1, 字段2) 
from [表名]
```


**==虚拟表==**
```
select 666*999 
from dual
```
- dual为虚拟表


**==不等号==**
- !=
- <>


**==查询某个区间的值==**
```
select * from [表名] 
where [字段] between [条件1] and [条件2]
```
- 条件1比条件2小
- 包含条件1和条件2
- 字符串用' '包起来


**==查询包含占位符(%或_)的信息==**
```
select * from [表名] 
where [字段] like '%a%%' escape('a')
```
- 查询含有%的结果
- a为特殊符号，表示a后面的第一个为普通字符，不是占位符
```
select * from [表名] 
where [字段] like '%aaa%%' escape('a')
```
- 查询含有a%的结果


**==查找字符函数==**
➣ instr
- select instr([字段]or[字符串], [查找的字符]) from [表名]
```
select instr('helloworld', 'l') from dual
```
- 返回结果为3
```
select instr('helloworld', 'l', 6) from dual
```
- 返回结果为9，从第6位开始查找'l'
```
select instr('helloworld', 'l', 1, 2) from dual
```
- 返回结果为4，从第1位开始查找，查找第二个出现的'l'


**==求字符串的长度==**
➣ length
- select length([字段名] or [字符串]) from [表名]
```
select length('Book') from dual
```
- 返回结果为4


**==字符串转换为小写==**
➣ lower
```
select lower([字段名] or [字符串]) from [表名]
```


**==字符串转换为大写==**
➣ upper
```
select upper([字段名] or [字符串]) from [表名]
```


**==去除字符串里的空格或特定字符==**
➣ LTRIM
```
select ltrim[字符串] from dual
```
- 去除字符串左边的空格
```
select ltrim('aaa123456bbb', 'aaa') 
from dual
```
- 去除aaa，结果为123456bbb

➣ RTRIM
```
select rtrim[字符串] from dual
```
- 去除字符串右边的空格
```
select rtrim('aaa123456bbb', 'bbb') 
from dual
```
- 去除bbb，结果为aaa123456


**==字符串替换==**
➣ replace
- select replace([字符串], [待替换的字符串], [需要替换的字符串]) from dual
```
select replace('a1a2', 'a', 'b') from dual
```
- 运行结果：b1b2


**==获取系统的当前时间==**
➣ sysdate
```
select sysdate from dual
```
➣ current_date
```
select current_date from dual
```



**==获取系统当前时间后一天的时间==**
➣ sysdate
```
select sysdate+1 from dual
```
➣ current_date
```
select current_date+1 from dual
```


==**获取某个特定日期加上几个月的日期**==
➣ add_monts
```
select add_months([字段], 2) 
from [表名]
```
- 2个月后的日期


**==获取某个日期月份的最后一天==**
➣ last_day
```
select last_day([字段]) from [表名]
```


**==获取两个日期之间的月份差==**
➣ months_between
```
select months_between([日期1], [日期2]) from [表名]
```
- 日期1 - 日期2的月份差


**==获取当前时间的下个星期的时间==**
➣ next_day
```
select next_day(sysdate, '星期一') from dual
```
- 星期一到星期日都可以识别


**==数据分组后筛选==**
➣ having
- 要和group by函数配对使用
```
select [字段1], [组函数] 
from [表名] 
group by [字段1] 
having [组函数的筛选条件]
```


**==利用rowid去重==**
➣ rowid是每条记录独有的id，不会重复
```
delete from copy 
where rowid not in
(
select min(rowid) 
from copy 
group by deptno, dname, loc
)
```
- copy为表名，deptno/dname/loc为字段名


**==创建表时添加约束，约束不指定名称==**
```
create table tb_user (
userid nunber(5) primary key,
username varchar2(30) check(lenth(username) between 4 and 20) not null,
age number(3) default(18) check(age>=18),
gender char(3) default('男') check(gender in ('男', '女')),
email varchar2(30) unique,
regtime date default(sysdate)
)
```
- userid为主键
- username长度在4到20，非空
- age默认值18，大于等于18
- gender默认值男，在男和女之间输入
- email唯一
- regtime默认值为当前时间

```
create table tb_txt 
(
txtid number(5) primary key,
txtid varchar(32) not null check(lenth(txtid)>=4 and lenth(txtid)<=30),
txt varchar2(1024),
pubtime date default(sysdate),
userid number(5) references tb_user(userid) on delete set null
)
```
- userid为外键，参考tb_user表的userid，删除时自设为nul


**==创建表时添加约束，指定约束名字==**
```
create tb_txt 
(
txtid number(5),
title varchar2(30) constraint nn_txt_title not null,
txt varchar2(1024),
pubtime date default(sysdate),
userid number(5),
constraint pk_txt_id primary key(txtid),
constraint ck_txt_title check(length(title) between 4 and 30),
constraint fk_txt_user_id foreign key(userid) references tb_user(userid) on delete set null
)
```
- constraint后面接的是自定义的约束名


**==追加约束==**
```
alter table tb_user 
add constraint pk_user_id primary key(userid)
```
- 添加主键约束
```
alter table tb_user 
add constraint ck_user_name check(length(username) between 4 and 20)
```
- 添加检查约束，username在4到20之间
```
alter table tb_user 
add constraint ck_user_gender check(gender in ('男', '女'))
```
- 添加检查约束，gender在男和女之间
```
alter table tb_user 
add constraint uq_user_email unique(email)
```
- 添加检查约束，email设为唯一
```
alter table tb_user 
modify(age default(18))
```
- age默认值设为18
```
alter table tb_user modify(username constraint nn_user_name not null)
```
- username设为非空
```
alter table tb_txt 
add constraint fk_txt_user_id foreign key(userid) references tb_user(userid)
```
- 外键约束，强制不让删
```
alter table tb_txt 
add constraint fk_txt_user_id foreign key(userid) references tb_user(userid) on delete set null
```
- 外键约束，自动设为null
```
alter table tb_user 
add constraint fk_txt_user_id foreign key(userid) references tb_user(userid) on delete cascade
```
- 外键约束，级联删除


**==删除约束==**
```
alter table tb_user 
drop constraint pk_user_id
```
- tb_user : 表名
- pk_user_id : 约束名


**==插入数据insert==**
```
insert into 表名 values(字段)
```
- 所有字段都要一一对应输入
```
insert into 表名1 value
(select * from 表名2)
```
- 从表2复制数据到表1
- 注意是value
```
insert into 表名1 value
(select 字段1, 字段2 
from 表名2 where 条件)
```
- 从表2复制数据到表1
- 注意是value


**==通过指定列的方式插入数据==**
```
insert into tb_user 
(userid, regtime, username, userpwd, email, gender, age) 
values (10002, to_date('1990-1-1', 'yyyy-mm-dd'), 'tomi', '1111', 'chengyingxu@lgcns.com', '男', 19)
```
- 字段值可以为null或者有默认值时，可以不指定
- 字段值有默认值时，则指定时使用默认值填充
- 字段值无默认值，但可以为null时，未指定时使用null填充
- 字段值可以为null，又有默认值，未指定时使用默认值填充，想要设为null时，需手动指定
```
insert into tb_user(userid, username, userpwd, regtime) (select empno, ename, ename, hiredate from emp where deptno=10)
```
- 通过指定列的方式，查询其他表的数据后，插入到此表中
- emp表中10部门的员工
- empno=userid, ename=username, ename=userpwd, hiretime=regtime


**==update更新记录用法==**
```
update tb_txt 
set txt = 'aaaa' 
where txtid = 3
```
- 更新一条记录
```
update tb_txt 
set txt = 'bbbb', userid = 0002 where txtid = 2
```
- 修改多条记录，一个字段一个字段修改
```
update tb_txt 
set(txt, userid) = (select 'ccc', 10001 from dual) 
where txtid = 3
```
- 修改多条记录，先列出要修改的字段，通过查询子句设定
- 从虚表dual中查询
```
update tb_txt 
set title = 'title0' 
where 1=1
```
- 更新所有title的值
- 一定要加where条件


**==限制返回集的行数==**
```
SELECT column
FROM table_name
FETCH FIRST 3 ROWS ONLY
```
- 返回前3行的数据


**==删除数据==**
```
delete from 表名 
where 条件
```

