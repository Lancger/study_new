# 一、配置ansible环境
```
cat > /etc/ansible/ansible.cfg <<EOF
[defaults]
inventory      = /etc/ansible/hosts
library        = /usr/share/my_modules/
module_utils   = /usr/share/my_module_utils/
remote_tmp     = ~/.ansible/tmp
local_tmp      = ~/.ansible/tmp
roles_path    = /opt/ansible/playbooks/roles
#关闭第一次使用ansible连接客户端是输入命令提示,关闭StrictHostKeyChecking检查
host_key_checking = False
deprecation_warnings=False
timeout = 10
log_path = /var/log/ansible.log
#smart 表示默认收集 facts，但 facts 已有的情况下不会收集，即使用缓存 facts
gathering = smart
fact_caching = jsonfile
fact_caching_connection=/tmp/.ansible_caching
[inventory]
[privilege_escalation]
[paramiko_connection]
[ssh_connection]
#开启SSH长连接
ssh_args = -C -o ControlMaster=auto -o ControlPersist=600s
control_path_dir = ~/.ansible/cp
control_path = %(directory)s/%%h-%%r
#默认情况下，ansible的执行流程是把生成好的本地python脚本PUT到远程服务器然后运行。如果开启了pipelining，整个流程少了一个PUT脚本到远程服务器的步骤，直接在SSH的会话中进行，可以提高整个执行效率。
pipelining = True
#scp将代替用来为远程主机传输文件
scp_if_ssh=False
[persistent_connection]
[accelerate]
[selinux]
[colors]
[diff]
EOF
```
# 二、ansible-playbook测试
```
#进入目录
cd /opt/ansible/playbooks/roles/node_exporter_install

#安装
ansible-playbook  node_exporter.yml --tags="install"

#卸载
ansible-playbook  node_exporter.yml --tags="uninstall"

#重启
ansible-playbook  node_exporter.yml --tags="restart"
```
