# 一、ansible-playbook测试
```
cd /opt/ansible/playbooks/roles/node_exporter_install

#安装
ansible-playbook  node_exporter.yml --tags="install"

#卸载
ansible-playbook  node_exporter.yml --tags="uninstall"

#重启
ansible-playbook  node_exporter.yml --tags="restart"
```
