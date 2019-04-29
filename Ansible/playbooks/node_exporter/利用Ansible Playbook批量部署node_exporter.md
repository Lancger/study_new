1、下载及解压安装包

```
cd /usr/local/src/
wget https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz

mkdir -p /data0/prometheus 
groupadd prometheus
useradd -g prometheus prometheus -d /data0/prometheus
 
tar -xvf node_exporter-0.17.0.linux-amd64.tar.gz
cd /usr/local/src/
mv node_exporter-0.17.0.linux-amd64 /data0/prometheus/node_exporter
 
chown -R prometheus.prometheus /data0/prometheus

```

2、play-book文件编写
```
- hosts: yourservers
  user: root
  gather_facts: false
  vars:
    - user: "prometheus"
    - group: "prometheus"
    - node_exporter_package: "node_exporter-0.16.0.linux-amd64"

  tasks:
  - group: name={{ group}} state=present

  - name: Add user prometheus
    user: name={{ user }} shell=/sbin/nologin

  - file: path=/usr/local/prometheus owner={{ user}} group={{ group }} mode=750 state=directory

  - name: Sync files
    copy: src={{ item.src }} dest={{ item.dest }} owner={{ user}} group={{ group }}
    with_items:
      - {src: "node_exporter.service", dest: "/usr/lib/systemd/system"}

  - name: Unpack package
    unarchive: src={{ node_exporter_package }}.tar.gz dest=/usr/local/prometheus owner={{ user }} group={{ group }} creates=/usr/local/prometheus/node_exporter

  - name: Rename the path
    command: mv /usr/local/prometheus/{{ node_exporter_package }} /usr/local/prometheus/node_exporter creates=/usr/local/prometheus/node_exporter

  - file: path=/usr/local/prometheus/node_exporter owner={{ user}} group={{ group }} mode=750

  - name: Start service prometheus, if not running
    service:
      name: node_exporter.service
      state: started
 ```
