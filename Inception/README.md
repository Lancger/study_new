## Inception
### 2.1 安装
```bash
yum -y install cmake libncurses5-dev libssl-dev g++ bison gcc gcc-c++ openssl-devel ncurses-devel mysql MySQL-python
wget http://ftp.gnu.org/gnu/bison/bison-2.5.1.tar.gz
tar -zxvf bison-2.5.1.tar.gz
cd bison-2.5.1
./configure
make
make install

cd /usr/local/
wget https://github.com/myide/inception/archive/master.zip
unzip master.zip
cd inception-master/
sh inception_build.sh builddir linux
```

### 2.2 修改配置
编辑文件 /etc/inc.cnf ,内容如下
```bash
tee /etc/inc.cnf <<-'EOF'
[inception]
general_log=1
general_log_file=inc.log
port=6669
socket=/tmp/inc.socket
character-set-client-handshake=0
character-set-server=utf8
inception_remote_system_password=123456
inception_remote_system_user=root
inception_remote_backup_port=3306
inception_remote_backup_host=127.0.0.1
inception_support_charset=utf8
inception_enable_nullable=0
inception_check_primary_key=1
inception_check_column_comment=1
inception_check_table_comment=1
inception_osc_min_table_size=1
inception_osc_bin_dir=/usr/bin
inception_osc_chunk_time=0.1
inception_ddl_support=1
inception_enable_blob_type=1
inception_check_column_default_value=1
EOF
```

## 2.3 启动服务
```bash
nohup /usr/local/inception-master/builddir/mysql/bin/Inception --defaults-file=/etc/inc.cnf &
```
