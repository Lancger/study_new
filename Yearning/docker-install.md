```
docker build -t yearning:base .

docker rm -f `docker ps -a -q`

docker run -d --name=yearning -v /data/Yearning/:/mnt/Yearning -p 8000:8000 -p 2222:22 yearning:base
```

```
FROM centos:latest

RUN yum install -y wget git zlib zlib-devel readline-devel sqlite-devel bzip2-devel openssl-devel gdbm-devel libdbi-devel ncurses-libs kernel-devel libxslt-devel libffi-devel python-devel mysql-devel zlib-devel sshpass libtool make

RUN cd /usr/local/src && wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py && python get-pip.py && pip install -U pip

ADD Yearning /mnt/Yearning/

RUN pip install -r /mnt/Yearning/requirements.txt && cd /mnt/Yearning/  && python manage.py makemigrations OpsManage && python manage.py makemigrations wiki && python manage.py makemigrations orders && python manage.py makemigrations filemanage && python manage.py migrate && python manage.py loaddata superuser.json
#CMD  bash /mnt/OpsManage/start.sh

RUN yum -y install openssh-server

RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key

RUN /bin/echo 'root:123456'|chpasswd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
```
```
start_yearning.sh

/usr/sbin/nginx
cd /mnt/src
sed -i "s/ipaddress =.*/ipaddress=$HOST/" deploy.conf
sed -i "s/address =.*/address=$MYSQL_ADDR/" deploy.conf
sed -i "s/username =.*/username=$MYSQL_USER/" deploy.conf
sed -i "s/password =.*/password=$MYSQL_PASSWORD/" deploy.conf
gunicorn settingConf.wsgi:application -b 0.0.0.0:8000 -w 2



FROM yearning_local
MAINTAINER cookieYe 2018-9-17
EXPOSE 8000
EXPOSE 80
ENTRYPOINT start_yearning.sh
```
