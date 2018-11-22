开源地址      https://github.com/mysql-inception/inception


文档地址      http://mysql-inception.github.io/inception-document/


Inception安装

下载解压

wget https://github.com/mysql-inception/inception.git

pwd

inception

unzip inception-master.zip

安装Inception

cd /inception/inception-master/

#执行如下命令，可以看到安装帮助

sh inception_build.sh

Usage: inception_build.sh builddir [platform(linux:Xcode)]

EXAPMLE: inception_build.sh debug [Xcode]

#安装到/data目录

sh inception_build.sh /data

#说明：

1.inception_build.sh实际上封装了cmake && make && make install编译安装的步骤

2.buliddir是安装目录，platform是所在平台（默认linux）

3.每次如果出错之后，需要把编译目录删除掉，重新执行，不然会执行出错。

4.编译过程没有err那说明安装成功了

配置Inception

#与MySQL类型，可以指定一个cnf配置文件

vim /etc/inc.cnf

[inception]

general_log=1

general_log_file=inception.log

port=6669

socket=/tmp/inc.socket

character-set-client-handshake=0

character-set-server=utf8

。。。。。。。

#此处只是简单的配置为了启动Inception服务，更多的配置选项请参考文档

启动Inception

cd /inception/inception-master/yk/mysql/bin

nohup /Inception --defaults-file=/etc/inc.cnf &

netstat -antpl|grep 6669

tcp        0      0 0.0.0.0:6669       0.0.0.0:*          LISTEN      4598/Inception   

至此，可以看到inception的6669端口已经运行
连接Inception

mysql -h127.0.0.1 -P6669

Welcome to the MySQL monitor.  Commands end with ; or \g.

Your MySQL connection id is 2

Server version: Inception2.1.23 1

……………………..

mysql>  inception get variables; #获取inception所有的变量和值

+------------------------------------------+----------------------------------------------------------+

| Variable_name             | Value                               |

+------------------------------------------+----------------------------------------------------------+

| autocommit               | OFF                                 |

| bind_address              | *                                   |

| character_set_system       | utf8                                 |

………………..
Inception简单使用

1、  Inception规定，在语句的最开始位置，要加上inception_magic_start;语句，在执行语句块的最后加上inception_magic_commit;语句，这2个语句在 Inception 中都是合法的、具有标记性质的可被正确解析的 SQL 语句。被包围起来的所有需要审核或者执行的语句都必须要在每条之后加上分号，其实就是批量执行SQL语句。（包括use database语句之后也要加分号，这点与 MySQL 客户端不同），不然存在语法错误

2、  目前执行只支持通过C/C++接口、Python接口来对Inception访问，这一段必须是一次性的通过执行接口提交给Inception，那么在处理完成之后，Inception会返回一个结果集，来告诉我们这些语句中存在什么错误，或者是完全正常等等

 

如下

/*--user=yujx;--password=xxxxxxxxxxx;--host=127.0.0.1;--enable-check;--port=3306;*/ 

inception_magic_start; 

use test; 

CREATE TABLE yujx(id int); 

inception_magic_commit;
下面是一段执行上面语句的Python程序的例子：

cat inc-mysql.py

#!/usr/bin/env python

#coding=utf8

import MySQLdb

sql='''/*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/\

    inception_magic_start;\

    use test;\

    CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试';\

    inception_magic_commit;'''

#print sql

try:

        conn=MySQLdb.connect(host='127.0.0.1',user='',passwd='',db='',port=6669)

        cur=conn.cursor()

        ret=cur.execute(sql)

        result=cur.fetchall()

        num_fields = len(cur.description)

        field_names = [i[0] for i in cur.description]

        print field_names

        for row in result:

                print row[0], "|",row[1],"|",row[2],"|",row[3],"|",row[4],"|",row[5],"|",row[6],"|",row[7],"|",row[8],"|",row[9],"|",row[10]

        cur.close()

        conn.close()

except MySQLdb.Error,e:

             print "Mysql Error %d: %s" % (e.args[0], e.args[1])


当创建表语句为
CREATE TABLE yujx(id int)


python inc-mysql.py

['ID', 'stage', 'errlevel', 'stagestatus', 'errormessage', 'SQL', 'Affected_rows', 'sequence', 'backup_dbname', 'execute_time', 'sqlsha1']

1 | CHECKED | 0 | Audit completed | None | use test | 0 | '0_0_0' | None | 0 |

2 | CHECKED | 1 | Audit completed | Set engine to innodb for table 'yujx'.

Set charset to one of 'utf8mb4' for table 'yujx'.

Set comments for table 'yujx'.

Column 'id' in table 'yujx' have no comments.

Column 'id' in table 'yujx' is not allowed to been nullable.

Set Default value for column 'id' in table 'yujx'

Set a primary key for table 'yujx'. | CREATE TABLE yujx(id int) | 0 | '0_0_1' | 127_0_0_1_3306_test | 0 |
CREATE TABLE yujx(id int) engine=innodb;

]$ python inc-mysql.py

['ID', 'stage', 'errlevel', 'stagestatus', 'errormessage', 'SQL', 'Affected_rows', 'sequence', 'backup_dbname', 'execute_time', 'sqlsha1']

1 | CHECKED | 0 | Audit completed | None | use test | 0 | '0_0_0' | None | 0 |

2 | CHECKED | 1 | Audit completed | Set charset to one of 'utf8mb4' for table 'yujx'.

Set comments for table 'yujx'.

Column 'id' in table 'yujx' have no comments.

Column 'id' in table 'yujx' is not allowed to been nullable.

Set Default value for column 'id' in table 'yujx'

Set a primary key for table 'yujx'. | CREATE TABLE yujx(id int) engine=innodb | 0 | '0_0_1' | 127_0_0_1_3306_test | 0 |


以此类推，直到如下完整的创建语句时，终于成功
CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试'

python inc-mysql.py

['ID', 'stage', 'errlevel', 'stagestatus', 'errormessage', 'SQL', 'Affected_rows', 'sequence', 'backup_dbname', 'execute_time', 'sqlsha1']

1 | RERUN | 0 | Execute Successfully | None | use test | 0 | '1464252128_35_0' | None | 0.000 |

2 | EXECUTED | 0 | Execute Successfully | None | CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试' | 0 | '1464252128_35_1' | 127_0_0_1_3306_test | 0.010 |

如上，是默认的inception审核规则，用户可以根据自己的实际情况来自定义某些规则
Inception操作LOG

tail -f /inception/inception-master/yk/mysql/bin/inception.log     

                   24 Query     /*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/    inception_magic_start;    use test;    CREATE TABLE yujx(id int) engine=innodb;    inception_magic_commit

 

160526 16:42:08    25 Query     set autocommit=0

 

                   25 Query     /*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/    inception_magic_start;    use test;    CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试';    inception_magic_commit

 

160526 17:13:50    26 Query     select @@version_comment limit 1

 

160526 17:13:54    26 Query     inception get variables
Inception所支持的参数变量

参考：http://mysql-inception.github.io/inception-document/variables/
Inception备份功能
相关说明

Inception在做DML操作时，具有备份功能，它会将所有当前语句修改的行备份下来，存储到一个指定的库中，这些库的指定需要通过几个参数，它们分别是：

inception_remote_backup_host //远程备份库的host

inception_remote_backup_port //远程备份库的port

inception_remote_system_user //远程备份库的一个用户

inception_remote_system_password //上面用户的密码

1.这些参数可以直接在命令行中指定，也可以在inc.cnf配置文件中指定。修改这些参数需要重启inception服务。

2.Inception默认是开启备份功能的，可以使用参数--disable-remote-backup=1来关闭备份功能

3.被影响的表如果没有主键的话，就不会做备份

4.Inception备份用户必须要具备CREATE、INSERT。CREATE权限用于创建表或者库的，INSERT权限用于插入备份数据的，其它的权限不需要（也许是暂时的）

5.备份数据在备份机器的存储，是与线上被修改库一对一的。但因为机器的多（线上机器有很多）对一（备份机器只有一台），所以为了防止库名的冲突，备份机器的库名组成是由线上机器的 IP 地址的点换成下划线，再加上端口号，再加上库名三部分，这三部分也是通过下划线连接起来的,如：10_103_11_242_3306_test

6.对线上配置需求

线上服务器必须要打开 binlog，在启动时需要设置参数log_bin、log_bin_index等关于 binlog 的参数。不然不会备份及生成回滚语句。

参数binlog_format必须要设置为 mixed 或者 row 模式，通过语句： set global binlog_format=mixed/row 来设置，如果是 statement 模式，则不做备份及回滚语句的生成。

参数 server_id 必须要设置为非0及非1，通过语句：set global server_id=server_id;来设置，不然在备份时会报错。

线上服务器一定要有指定用户名的权限，这个是在语句前面的注释中指定的，做什么操作就要有什么权限，否则还是会报错，如果需要执行的功能，则要有线上执行语句的权限，比如DDL及DML，同时如果要执行inception show 等远程命令的话，有些语句是需要特殊权限的，这些权限也是需要授予的，关于权限这个问题，因为一般Inception是运行在一台固定机器上面的，那么在选项中指定的用户名密码，实际上是Inception机器对线上数据库访问的权限，所以建议在使用过程中，使用专门固定的帐号来让Inception使用，最好是一个只读一个可写的即可，这样在执行时用可写，审核或者查看线上状态或者表库结果时用只读即可，这样更安全。

需要额外注意的点

在执行时，不能将 DML 语句及 DDL 语句放在一起执行，否则会因为备份解析binlog时由于表结构的变化出现不可预知的错误，如果要有同时执行 DML 及 DDL，则请分开多个语句块儿来执行，如果真的这样做了，Inception 会报错，不会去执行。
备份功能试验
备份服务器创建bakcup账号并且grant权限

#由于测试环境就开all权限了

mysql> grant all on *.* to 'backup'@'%' identified by 'backup';

mysql> flush privileges;
指定备份服务器信息

grep remote /etc/inc.cnf

inception_remote_system_password=backup

inception_remote_system_user=backup

inception_remote_backup_port=3306

inception_remote_backup_host=10.10.69.241
重启inception

]$Kill -9 inception_pid

]$ nohup /Inception --defaults-file=/etc/inc.cnf &
确保线上目标库binlog打开&&binlog_format

mysql> show variables like 'log_bin';        

+---------------+-------+

| Variable_name | Value |

+---------------+-------+

| log_bin       | ON    |

+---------------+-------+

1 row in set (0.00 sec)

 

mysql> show variables like 'binlog_format';       

+---------------+-------+

| Variable_name | Value |

+---------------+-------+

| binlog_format | ROW   |

+---------------+-------+

1 row in set (0.00 sec)
调用inception操作目标库

python inc-mysql.py

['ID', 'stage', 'errlevel', 'stagestatus', 'errormessage', 'SQL', 'Affected_rows', 'sequence', 'backup_dbname', 'execute_time', 'sqlsha1']

1 | RERUN | 0 | Execute Successfully | None | use test | 0 | '1464320865_11_0' | None | 0.000 |

2 | EXECUTED | 0 | Execute Successfully

Backup successfully | None | CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试' | 0 | '1464320865_11_1' | 127_0_0_1_3306_test | 0.010 |
查看备份库

#如下，备份库多出了2个db:inception和IP_PORT_DBNAME

mysql> show databases;

+---------------------+

| Database            |

+---------------------+

| information_schema  |

| 127_0_0_1_3306_test |

| inception           |

| mysql               |

| performance_schema  |

| test                |

+---------------------+

#inception.statistic表存储的就是SQL执行数目的统计数据

参考：http://mysql-inception.github.io/inception-document/statistic/

clip_image002

#IP_PORT_dbname库下表中记录了对应的rollback操作

clip_image004

除了与原库中的表一一对应之外，每个备份库中还有另外一个表：$_$Inception_backup_information$_$，这就是表名，非常难看，主要是为了防止与原线上库中的表名发生冲突。这个表是用来存储所有对当前库操作的操作记录的，它是公用的，对线上这个库的所有表操作记录都存储在这里面

参考链接：http://mysql-inception.github.io/inception-document/backup/
Inception OSC功能

参考：http://mysql-inception.github.io/inception-document/osc/
配置PT-osc的路径

]$ which pt-online-schema-change

/usr/bin/pt-online-schema-change

]$ grep bin_dir /etc/inc.cnf   

inception_osc_bin_dir=/usr/bin/pt-online-schema-change
设置osc相关选项

]$ grep osc inc-mysql-osc.py

inception set session inception_osc_recursion_method=none;

inception set session inception_osc_alter_foreign_keys_method=0;

inception set session inception_osc_min_table_size=1; #设置表大小超过此参数的值时，inception调用osc工具alter表

执行inception调用pt-osc

]$ python inc-mysql-osc.py

['ID', 'stage', 'errlevel', 'stagestatus', 'errormessage', 'SQL', 'Affected_rows', 'sequence', 'backup_dbname', 'execute_time', 'sqlsha1']

1 | RERUN | 0 | Execute Successfully | None | use test | 0 | '1464335578_270_0' | None | 0.000 |

2 | EXECUTED | 0 | Execute Successfully

Backup successfully | None | alter table test add a int not null default 0 comment 'a字段' | 1 | '1464335727_270_1' | 127_0_0_1_3306_test | 149.880 | *8388D07BAD14866D80DD45CD1521F4F0D17C20CE
查看osc的进度

#查看osc列表

mysql> inception get osc processlist;

#由上面的processlist可以看到正在执行的osc任务对应的SQLSHA1值，然后使用下方命令可以查看指定的一条osc任务进度

mysql> inception get osc_percent '*8388D07BAD14866D80DD45CD1521F4F0D17C20CE';

+--------+-----------+-------------------------------------------+---------+------------+-----------------------------------

| DBNAME | TABLENAME | SQLSHA1                                   | PERCENT | REMAINTIME | INFOMATION                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

+--------+-----------+-------------------------------------------+---------+------------+----------------------------------------------------------------------------------------| test   | test      | *8388D07BAD14866D80DD45CD1521F4F0D17C20CE |      92 | 00:11      | Operation, tries, wait:

  copy_rows, 10, 0.25

  create_triggers, 10, 1

  drop_triggers, 10, 1

  swap_tables, 10, 1

  update_foreign_keys, 10, 1

No foreign keys reference `test`.`test`; ignoring --alter-foreign-keys-method.

Altering `test`.`test`...

Creating new table...

CREATE TABLE `test`.`_test_new` (  `x` int(11) DEFAULT NULL,

  `y` int(11) DEFAULT NULL,

  `z` int(11) NOT NULL AUTO_INCREMENT,

  PRIMARY KEY (`z`)

) ENGINE=InnoDB AUTO_INCREMENT=6488605 DEFAULT CHARSET=latin1

Created new table test._test_new OK.

Altering new table...

ALTER TABLE `test`.`_test_new` add a int not null default 0 comment 'a?-???μ'

Altered `test`.`_test_new` OK.

Creating triggers...

CREATE TRIGGER `pt_osc_test_test_del` AFTER DELETE ON `test`.`test` FOR EACH ROW DELETE IGNORE FROM `test`.`_test_new` WHERE `test`.`_test_new`.`z` <=> OLD.`z`

CREATE TRIGGER `pt_osc_test_test_upd` AFTER UPDATE ON `test`.`test` FOR EACH ROW REPLACE INTO `test`.`_test_new` (`x`, `y`, `z`) VALUES (NEW.`x`, NEW.`y`, NEW.`z`)

CREATE TRIGGER `pt_osc_test_test_ins` AFTER INSERT ON `test`.`test` FOR EACH ROW REPLACE INTO `test`.`_test_new` (`x`, `y`, `z`) VALUES (NEW.`x`, NEW.`y`, NEW.`z`)

Created triggers OK.

INSERT LOW_PRIORITY IGNORE INTO `test`.`_test_new` (`x`, `y`, `z`) SELECT `x`, `y`, `z` FROM `test`.`test` FORCE INDEX(`PRIMARY`) WHERE ((`z` >= ?)) AND ((`z` <= ?)) LOCK IN SHARE MODE /*pt-online-schema-change 12934 copy nibble*/

SELECT /*!40001 SQL_NO_CACHE */ `z` FROM `test`.`test` FORCE INDEX(`PRIMARY`) WHERE ((`z` >= ?)) ORDER BY `z` LIMIT ?, 2 /*next chunk boundary*/
查看目标库结果

mysql> desc test;

+-------+---------+------+-----+---------+----------------+

| Field | Type    | Null | Key | Default | Extra          |

+-------+---------+------+-----+---------+----------------+

| x     | int(11) | YES  |     | NULL    |                |

| y     | int(11) | YES  |     | NULL    |                |

| z     | int(11) | NO   | PRI | NULL    | auto_increment |

| a     | int(11) | NO   |     | 0       |                |   #添加成功

+-------+---------+------+-----+---------+----------------+

4 rows in set (0.01 sec)
Inception 审核规范及原则

参考http://mysql-inception.github.io/inception-document/rules/

下面所列出来的规则，不一定能覆盖所有Inception当前已经实现的功能，具体包括什么规则，还需要在使用过程中总结，发现，同时可以结合配置参数来详细了解这些规则。
支持的语句类型

l   use db：此时会检查这个库是不是存在，需要连接到线上服务器来判断。

l   set option：现在只需要支持set names charset，设置其它变量时都报错不支持。

l   创建数据库语句

l   插入语句（包括多值插入）

l   查询插入语句

l   删除语句（包括多表删除）

l   更新语句（包括多表更新）

l   创建表语句

l   删除表语句

l   修改表语句

l   Truncate表语句

l   inception命令集语句（包括管理命令）
插入语句检查项

l   表是否存在

l   必须指定插入列表，也就是要对哪几个列指定插入值，如insert into t (id,id2) values(...)，（可配置）

l   必须指定值列表，与上面对应的列，插入的值是什么，必须要指定。

l   插入列列表与值列表个数相同，上面二者的个数需要相同，如果没有指定列列表（因为可配置），则值列表长度要与表列数相同。

l   不为null的列，如果插入的值是null，报错（可配置）

l   插入指定的列名对应的列必须是存在的。

l   插入指定的列列表中，同一个列不能出现多次。

l   插入值列表中的简单表达式会做检查，但具体包括什么不一一指定
更新、删除语句检查项

l   表是否存在

l   必须有where条件（可配置）

l   delete语句不能有limit条件（可配置）

l   不能有order by语句（可配置）

l   影响行数大于10000条，则报警（数目可配置）

l   对WHERE条件这个表达式做简单检查，具体包括什么不一一指定

l   对更新列的值列表表达式做简单检查，具体不一一指定

l   对更新列对象做简单检查，主要检查列是不是存在等

l   多表更新、删除时，每个表必须要存在
建表语句检查项
表属性的检查项

l   这个表不存在

l   对于create table like，会检查like的老表是不是存在。

l   对于create table db.table，会检查db这个数据库是不是存在

l   表名、列名、索引名的长度不大于64个字节

l   如果建立的是临时表，则必须要以tmp为前缀

l   必须要指定建立innodb的存储引擎（可配置）

l   必须要指定utf8的字符集（字符串可配置，指定支持哪些字符集）

l   表必须要有注释（可配置）

l   表不能建立为分区表（可配置）

l   只能有一个自增列

l   索引名字不能是Primay

l   不支持Foreign key（可配置）

l   建表时，如果指定auto_increment的值不为1，报错（可配置）

l   如果自增列的名字不为id，说明有可能是有意义的，MySQL这样使用比较危险，所以报警（可配置）
列属性的检查项

l   不能设置列的字符集（可配置）

l   列的类型不能使用集合、枚举、位图类型。（可配置）

l   列必须要有注释（可配置）

l   char长度大于20的时候需要改为varchar（长度可配置）

l   列的类型不能是BLOB/TEXT。（可配置）

l   每个列都使用not null（可配置）

l   如果列为BLOB/TEXT类型的，则这个列不能设置为NOT NULL。

l   如何是自增列，则使用无符号类型（可配置）

l   如果自增列，则长度必须要大于等于4个字节（可配置）

l   如果是timestamp类型的，则要必须指定默认值。

l   对于MySQL5.5版本（包含）以下的数据库，不能同时有两个TIMESTAMP类型的列，如果是DATETIME类型，则不能定义成DATETIME DEFAULT CURRENT_TIMESTAMP及ON UPDATE CURRENT_TIMESTAMP等语句。

l   每个列都需要定义默认值，除了自增列、主键列及大字段列之外（可配置）

l   不能有重复的列名
索引属性检查项

l   索引必须要有名字

l   不能有外键（可配置）

l   Unique索引必须要以uniq_为前缀（可配置）

l   普通索引必须要以idx_为前缀（可配置）

l   索引的列数不能超过5个（数目可以配置）

l   表必须要有一个主键（可配置）

l   最多有5个索引（数目可配置）

l   建索引时，指定的列必须存在。

l   索引中的列，不能重复

l   BLOB列不能建做KEY

l   索引长度不能超过766

l   不能有重复的索引，名字及内容
默认值检查项

l   BLOB/TEXT类型的列，不能有非NULL的默认值

l   MySQL5.5以下（含）的版本，对于DATETIME类型的列，不能有函数NOW()的默认值。

l   如果设置默认值为函数，则只能是NOW()。

l   如果默认值为NULL，但列类型为NOT NULL，或者是主键列，或者定义为自增列，则报错。

l   自增列不能设置默认值。

l   修改表语句检查项

l   表是不是存在
创建索引检查项

l   同上面创建表中的索引检查项
加列检查项

l   同上面创建表中的列检查项
修改表检查项

l   表是不是存在

l   如果语句块中存在多条对同一个表的修改语句，则建议合并成一个ALTER语句

l   列是否存在

l   剩下的同上面创建表，创建索引，创建列，默认值等检查项一样
删除索引检查项

l   表是不是存在

l   检查索引是不是存在
修改列的默认值检查项

l   同默认值检查项
修改表属性

l   表属性只支持对存储引擎、表注释、自增值及默认字符集的修改操作。

l   修改存储引擎时检查是不是Innodb（可配置）。

l   字符集修改检查是不是属于设置参数的值（支持字符集可配置）。
转换表字符集

l   字符集修改检查是不是属于设置参数的值（支持字符集可配置）。
声明

针对线上MySQL服务器是不是5.6以上（包含）版本，有不同的处理策略，比如在预估影响行数时，5.6可以直接对任何DML语句做EXPLAIN操作，而在5.5版本中，只支持对SELECT语句执行EXPLAIN操作，而在5.5版本中，有些DML语句是不容易直接转换为SELECT语句去做EXPLAIN，这样导致预估行数为0。

还是针对5.6以上版本与5.5版本的不同，DATETIME、TIMESTAMP系列类型在执行时，5.5的限制比较多，而5.6基本通用，所以这上面的处理可能在5.6及5.5版本上，相同语句返回的结果集是不同的（规则是在5.5中以在执行时报的错误信息为准），这与线上版本有关系。
附：执行的三个脚本
创建表

cat inc-mysql.py    

#!/usr/bin/env python

#coding=utf8

import MySQLdb

sql='''/*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/\

    inception_magic_start;\

    use test;\

    CREATE TABLE yujx(id int comment 'test' primary key) engine=innodb DEFAULT CHARSET=utf8mb4 comment '测试';\

    inception_magic_commit;'''

#print sql

try:

        conn=MySQLdb.connect(host='127.0.0.1',user='',passwd='',db='',port=6669)

        cur=conn.cursor()

        ret=cur.execute(sql)

        result=cur.fetchall()

        num_fields = len(cur.description)

        field_names = [i[0] for i in cur.description]

        print field_names

        for row in result:

                print row[0], "|",row[1],"|",row[2],"|",row[3],"|",row[4],"|",row[5],"|",row[6],"|",row[7],"|",row[8],"|",row[9],"|",row[10]

        cur.close()

        conn.close()

except MySQLdb.Error,e:

             print "Mysql Error %d: %s" % (e.args[0], e.args[1])

Insert表

cat inc-mysql-insert.py

#!/usr/bin/env python

#coding=utf8

import MySQLdb

sql='''/*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/\

    inception_magic_start;\

    use test;\

    insert into yujx(id) value(1);\

    inception_magic_commit;'''

#print sql

try:

        conn=MySQLdb.connect(host='127.0.0.1',user='',passwd='',db='',port=6669)

        cur=conn.cursor()

        ret=cur.execute(sql)

        result=cur.fetchall()

        num_fields = len(cur.description)

        field_names = [i[0] for i in cur.description]

        print field_names

        for row in result:

                print row[0], "|",row[1],"|",row[2],"|",row[3],"|",row[4],"|",row[5],"|",row[6],"|",row[7],"|",row[8],"|",row[9],"|",row[10]

        cur.close()

        conn.close()

except MySQLdb.Error,e:

             print "Mysql Error %d: %s" % (e.args[0], e.args[1])

OSC表

cat inc-mysql-osc.py

#!/usr/bin/env python

#coding=utf8

import MySQLdb

sql='''

        inception set session inception_osc_recursion_method=none;

        inception set session inception_osc_alter_foreign_keys_method=0;

        inception set session inception_osc_min_table_size=1;

 

  /*--user=yujx;--password=yujx;--host=127.0.0.1;--execute=1;--port=3306;*/\

    inception_magic_start;\

    use test;\

    alter table test add b int not null default 0 comment 'b字段';\

    inception_magic_commit;'''

try:

        conn=MySQLdb.connect(host='127.0.0.1',user='',passwd='',db='',port=6669)

        cur=conn.cursor()

        ret=cur.execute(sql)

        result=cur.fetchall()

        num_fields = len(cur.description)

        field_names = [i[0] for i in cur.description]

        print field_names

        for row in result:

                print row[0], "|",row[1],"|",row[2],"|",row[3],"|",row[4],"|",row[5],"|",row[6],"|",row[7],"|",row[8],"|",row[9],"|",row[10]

        cur.close()

        conn.close()

except MySQLdb.Error,e:

             print "Mysql Error %d: %s" % (e.args[0], e.args[1]) 
             
             
 参考文档：
 
 http://blog.itpub.net/27000195/viewspace-2120332/    Inception相关功能学习 
