安装Inception
```
# wget https://www.percona.com/redir/downloads/percona-release/redhat/percona-release-0.1-3.noarch.rpm
# rpm -Uvh percona-release-0.1-3.noarch.rpm 
# yum install percona-toolkit
# yum install cmake bison  ncurses-devel gcc gcc-c++  openssl-devel
# cd /data
# mkdir /usr/local/inception/data -p      --这里创建的是Inception的安装目录。可根据自己的情况自行决定
# mkdir inception
# cd inception
# wget https://github.com/mysql-inception/inception/archive/master.zip
# unzip master
# cd inception-master/  
# cmake .
# cmake -DWITH_DEBUG=OFF -DCMAKE_INSTALL_PREFIX=/usr/local/inception  -DMYSQL_DATADIR=/data/inception -DWITH_SSL=yes -DCMAKE_BUILD_TYPE=RELEASE-DWITH_ZLIB=bundled-DMY_MAINTAINER_CXX_WARNINGS="-Wall -Wextra -Wunused -Wwrite-strings -Wno-strict-aliasing  -Wno-unused-parameter -Woverloaded-virtual" -DMY_MAINTAINER_C_WARNINGS="-Wall -Wextra -Wunused -Wwrite-strings -Wno-strict-aliasing -Wdeclaration-after-statement"
# make && make install
# vim /etc/inc.cnf

[inception]
general_log=1 #这个参数就是原生的MySQL的参数，用来记录在Inception服务上执行过哪些语句，用来定位一些问题等
general_log_file=/usr/local/inception/data/inception.log #设置general log写入的文件路径
port=6669   #Inception的服务端口
socket=/usr/local/inception/inc.socket #Inception的套接字文件存放位置
character-set-server=utf8 #mysql原生参数

#Inception 审核规则
inception_check_autoincrement_datatype=1 #当建表时自增列的类型不为int或者bigint时报错
inception_check_autoincrement_init_value=1 #当建表时自增列的值指定的不为1，则报错
inception_check_autoincrement_name=1 #建表时，如果指定的自增列的名字不为ID，则报错，说明是有意义的，给提示
inception_check_column_comment=1 #建表时，列没有注释时报错
inception_check_column_default_value=0 #检查在建表、修改列、新增列时，新的列属性是不是要有默认值
inception_check_dml_limit=1 #在DML语句中使用了LIMIT时，是不是要报错
inception_check_dml_orderby=1 #在DML语句中使用了Order By时，是不是要报错
inception_check_dml_where=1 #在DML语句中没有WHERE条件时，是不是要报错
inception_check_identifier=1 #打开与关闭Inception对SQL语句中各种名字的检查，如果设置为ON，则如果发现名字中存在除数字、字母、下划线之外的字符时，会报Identifier "invalidname" is invalid, valid options: [a-z,A-Z,0-9,_].
inception_check_index_prefix=1 #是不是要检查索引名字前缀为"idx_"，检查唯一索引前缀是不是"uniq_"
inception_check_insert_field=1  #是不是要检查插入语句中的列链表的存在性
inception_check_primary_key=1 #建表时，如果没有主键，则报错
inception_check_table_comment=1 #建表时，表没有注释时报错
inception_check_timestamp_default=0 #建表时，如果没有为timestamp类型指定默认值，则报错
inception_enable_autoincrement_unsigned=1 #自增列是不是要为无符号型
inception_enable_blob_type=0 #检查是不是支持BLOB字段，包括建表、修改列、新增列操作 默认开启
inception_enable_column_charset=0 #允许列自己设置字符集
inception_enable_enum_set_bit=0 #是不是支持enum,set,bit数据类型
inception_enable_foreign_key=0 #是不是支持外键
inception_enable_identifer_keyword=0 #检查在SQL语句中，是不是有标识符被写成MySQL的关键字，默认值为报警。
inception_enable_not_innodb=0 #建表指定的存储引擎不为Innodb，不报错
inception_enable_nullable=0 #创建或者新增列时如果列为NULL，不报错
inception_enable_orderby_rand=0 #order by rand时是不是报错
inception_enable_partition_table=0 #是不是支持分区表
inception_enable_select_star=0 #Select*时是不是要报错
inception_enable_sql_statistic=1 #设置是不是支持统计Inception执行过的语句中，各种语句分别占多大比例，如果打开这个参数，则每次执行的情况都会在备份数据库实例中的inception库的statistic表中以一条记录存储这次操作的统计情况，每次操作对应一条记录，这条记录中含有的信息是各种类型的语句执行次数情况。
inception_max_char_length=16 #当char类型的长度大于这个值时，就提示将其转换为VARCHAR
inception_max_key_parts=5 #一个索引中，列的最大个数，超过这个数目则报错
inception_max_keys=16 #一个表中，最大的索引数目，超过这个数则报错
inception_max_update_rows=10000 #在一个修改语句中，预计影响的最大行数，超过这个数就报错
inception_merge_alter_table=1 #在多个改同一个表的语句出现是，报错，提示合成一个

#inception 支持 OSC 参数
inception_osc_bin_dir=/usr/bin/pt-online-schema-change #用于指定pt-online-schema-change脚本的位置，不可修改，在配置文件中设置
inception_osc_check_interval=5 #对应OSC参数--check-interval，意义是Sleep time between checks for --max-lag.
inception_osc_chunk_size=1000 #对应OSC参数--chunk-size
inception_osc_chunk_size_limit=4 #对应OSC参数--chunk-size-limit
inception_osc_chunk_time=0.1 #对应OSC参数--chunk-time
inception_osc_critical_thread_connected=1000 #对应参数--critical-load中的thread_connected部分
inception_osc_critical_thread_running=80 #对应参数--critical-load中的thread_running部分
inception_osc_drop_new_table=1 #对应参数--[no]drop-new-table
inception_osc_drop_old_table=1 #对应参数--[no]drop-old-table
inception_osc_max_lag=3 #对应参数--max-lag
inception_osc_max_thread_connected=1000 #对应参数--max-load中的thread_connected部分
inception_osc_max_thread_running=80 #对应参数--max-load中的thread_running部分
inception_osc_min_table_size=1 # 这个参数实际上是一个OSC的开关，如果设置为0，则全部ALTER语句都走OSC，如果设置为非0，则当这个表占用空间大小大于这个值时才使用OSC方式。单位为M，这个表大小的计算方式是通过语句："select (DATA_LENGTH + INDEX_LENGTH)/1024/1024 from information_schema.tables where table_schema = 'dbname' and table_name = 'tablename'"来实现的
inception_osc_on=1 #一个全局的OSC开关，默认是打开的，如果想要关闭则设置为OFF，这样就会直接修改
inception_osc_print_none=1 #用来设置在Inception返回结果集中，对于原来OSC在执行过程的标准输出信息是不是要打印到结果集对应的错误信息列中，如果设置为1，就不打印，如果设置为0，就打印。而如果出现错误了，则都会打印
inception_osc_print_sql=1 #对应参数--print
#inception_user #这个用户名在配置之后，在连接Inception的选项中可以不指定user，这样线上数据库的用户名及密码就可以不暴露了，可以做为临时使用的一种方式，但这个用户现在只能是用来审核，也就是说，即使在选项中指定--enable-execute，也不能执行，这个是只能用来审核的帐号。
#inception_password #与上面的参数是一对，这个参数对应的是选项中的password，设置这个参数之后，可以在选项中不指定password
inception_read_only=0 #设置当前Inception服务器是不是只读的，这是为了防止一些人具有修改权限的帐号时，通过Inception误修改一些数据，如果inception_read_only设置为ON，则即使开了enable-execute，同时又有执行权限，也不会去执行，审核完成即返回

#备份服务器信息
inception_remote_system_password=123456
inception_remote_system_user=root
inception_remote_backup_port=3306
inception_remote_backup_host=192.168.88.54
inception_support_charset=utf8 #表示在建表或者建库时支持的字符集，如果需要多个，则用逗号分隔，影响的范围是建表、设置会话字符集、修改表字符集属性等


# nohup /usr/local/inception/bin/Inception --defaults-file=/etc/inc.cnf &
# mysql -uroot -h127.0.0.1 -P6669
mysql> inception get variables;
```
