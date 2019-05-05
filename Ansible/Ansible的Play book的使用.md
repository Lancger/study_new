# 一、Ansible的Play book的使用

1、playbook简介

    playbook是ansible用于配置、部署和管理被控节点的剧本，通过playbook的详细描述，执行其中的一系列tasks，可以让远程主机达到预期的状态，playbook就像Ansible控制器给被控节点列出的一系列to-do-list，而被控节点必须要完成。

    也可以这样理解，pplaybook是由一个或多个"play"组成的列表。play的主要功能在于将事先归并为一组的主机装扮成事先通过ansible中的task定义好的角色。从根本上来讲所谓task无非是调用ansible的一个module。将多个play组织在一个playbook中即可以让它们联同起来按事先编排的机制同唱一台大戏。

2、playbook使用场景

    执行一些简单的任务，使用ad-hoc命令可以方便的解决问题，但是有时一个设施太过于复制，需要大量的操作时，执行ad-hoc命令就不太合适，这时最好使用playbook，就像执行shell命令与写shell脚本一样，也可以理解为批处理任务，不过playbook有自己的语法格式，具体可以看下面介绍。

    使用playbook你可以方便的重用这些代码，可以移值到不同的机器上面，像函数一样，最大化的利用代码。在使用ansible的过程中，所处理的大部分操作都是需要编写playbook的。

3、Playbook的核心元素

         Hosts：               主机，部署目标
         Tasks：               任务，ansible，执行目的
         Varlables：         变量
         Templates：       包含了模板语法的文本文件；
         Handlers：                   有特定条件触发的任务
         Roles ：              角色  (特别介绍)

4、playbook使用

	Usage: ansible-playbook playbook.yml

相对于ansible，增加了下列选项：

参数   说明

	--flush-cache 清除fact缓存
	--force-handlers  如果任务失败，也要运行handlers
	--list-tags   列出所有可用的标签
	--list-tasks  列出将要执行的所有任务
	--skip-tags=SKIP_TAGS    跳过运行标记此标签的任务
	--start-at-task=START_AT_TASK   在此任务处开始运行
	--step 一步一步：在运行之前确认每个任务
	-t TAGS, --tags=TAGS 只运行标记此标签的任务

示例：

	ansible-playbook -i hosts ssh-addkey.yml                # 指定主机清单文件
	ansible-playbook -i hosts ssh-addkey.yml  --list-tags   # 列出tags
	ansible-playbook -i hosts ssh-addkey.yml  -T install    # 执行install标签的任务

5、playbook的基础组件

	hosts: 运行指定任务的目标主机   
	remote_user：在远程主机以哪个用户身份执行；
	sudo_user：非管理员需要拥有sudo权限；
	tasks：任务列表
	  模块，模块参数,格式有如下两种：
	    (1) action: module arguments
	    (2) module: arguments 

示例1：

	- hosts: all
	remote_user: root
	tasks:
	 - name: install a group
	   group: name=mygrp system=true 
	 - name: install a user
	   user: name=user1 group=mygrp system=true

示例2

	- hosts: websrvs
	remote_user: root
	tasks:
	 - name: install httpd package
	  yum: name=httpd
	 - name: start httpd service 
	  service: name=httpd state=started

主要由三个部分组成。

    hosts部分：使用hosts指示使用哪个主机或主机组来运行下面的tasks，每个playbook都必须指定hosts，hosts也可以使用通配符格式。主机或主机组在inventory清单中指定，可以使用系统默认的/etc/ansible/hosts，也可以自己编辑，在运行的时候加上-i选项，指定清单的位置即可。在运行清单文件的时候，--list-hosts选项会显示那些主机将会参与执行task的过程中。
    remote_user：指定远端主机中的哪个用户来登录远端系统，在远端系统执行task的用户，可以任意指定，也可以使用sudo，但是用户必须要有执行相应task的权限。
    tasks：指定远端主机将要执行的一系列动作。tasks的核心为ansible的模块，前面已经提到模块的用法。tasks包含name和要执行的模块，name是可选的，只是为了便于用户阅读，不过还是建议加上去，模块是必须的，同时也要给予模块相应的参数。

6、Play book变量的使用

	（1）facts: 可直接调用
	（2）ansible-playbook 命令的命令行中的自定义变量
	    -e EXTRA_VARS, --extra-vars=EXTRA_VARS  #命令行中定义变量传递至yaml文件。
	（3）通过roles传递变量
	（4）Host Inventory
	（a）向不同的主机传递不同的变量；
	    IP/HOSTANME varable=value var2=value2
	    在hosts 组ip后添加变量
	（b）向组中的主机传递相同的变量
	    [group:var]         
	    arable=value
	注意：Inventory参数：
	 用于定义ansible远程连接目标主机时使用的参数，而非传递给playbook的变量。
	    ansible_ssh_host   
	    ansible_ssh_user
	    ansible_ssh_port
	    ansible_ssh_pass
	    ansible_sudo_pass
			….
	查看远程主机的全部系统信息
	ansible all -m setup  #收集到的远程主机的变量

(1)变量的定义示例:

	变量定义位置 /etc/ansible/hosts
	普通变量
		[web]
		172.16.250.240  http_port=80
		172.16.252.18   http_port=8080  
	组变量
		[web:var1]
		http_port=80
		[web]
		172.16.250.240  
		172.16.252.18    
	在playbook中定义变量的方法
		Vars：
	    - var1：value1
	    - var2：value2
	命令行指定变量
		nsible-playbook -e  调用

示例1：hosts定义变量使用方法

	[root@centos7_1 ~]#vim /etc/ansible/hosts
	[web]
	172.16.250.90 hname=node1
	[root@centos7_1 ~]# cd /apps/yaml/
	[root@centos7_1 yaml]# vim hosname.yml
	---
	- hosts: web
	  remote_user: root
	  tasks:
	  - name: sethostname
	    hostname:name={{ hname }}
	[root@centos7_1 yaml]# ansible-playbook  hosname.yml

示例2：在playbook中定义变量的方法

	[root@centos7_1yaml]# vim user1.yml
	---
	- hosts: web
	 remote_user: root
	 vars:  #定义变量
	 - username: testuser1   #变量列表
	 - groupname: testgroup1
	 tasks:
	 - name: crete group
	   group: name={{ groupname }} state=present
	 - name: crate user
	   user: name={{ username }} state=present                                                                                                                                                           
	[root@centos7_1 yaml]#ansible-playbook  user1.yml

示例3：命令行参数传递
利用命令行定义变量传递参数至剧本安装memcached。

	[root@centos7_1 yaml]#vim forth.yml
	---
	- hosts: web
	 remote_user: root
	 tasks:
	 - name: install $pkname
	yum: name={{pkname }} state=present       
	[root@centos7_1yaml]# ansible-playbook -e pkname=memcached forth.yml

7、Play book中notifyh和handlers的使用

notify这个action可用于每个play的最后被触发，这样可以避免多次有改变发生时每次都执行指定的操作，取而代之，仅在所有的变化发生完成之后一次性地执行指定操作。

在notify中列出的操作称为handler，即notify中调用handler中定义的操作。

handler也是也写task的列表，通过名字来引用，他们和一般的task没有什么区别。

handler是由通知者进行notify，如果没有被notify，handler是不会执行的。

不管有多少个通知者进行了notify，等到play中的所有task执行完成之后，handler也只会被执行一次。

handlers最佳的应用场景是用来重启服务，或者触发系统重启操作的。除此之外很少会用到的。


示例：触发

利用notify、handlers触发式重启服务。

	[root@centos7_1yaml]# vim web-2.yml
	---
	- hosts: web
	  remote_user: root
	  tasks:
	  - name: install httpdpackage
	    yum: name=httpdstate=present
	  - name: install configurefile
	copy: src=/apps/work/files/httpd.confdest=/etc/httpd/conf/ 
	#该文件与目标主机文件不完全一致变回触发。
	    notify: restart httpd
	  - name: start httpd service
	    service: name=httpdstate=started
	  handlers:
	  - name: restart httpd
	service: name=httpd state=restarted
	[root@centos7_1 yaml]#ansible-playbook  web-2.yml

8、Play book中tags的使用

    tags即标签，tags可以和一个play（就是很多个task）或者一个task进行捆绑。然后再执行play book时只需指定相应的tags即可仅执行与tags绑定的task。

示例：执行指定tags

	[root@centos7_1yaml]# vim web-3.yml
	---
	- hosts: web
	 remote_user: root
	 tasks:
	 - name: install httpd package
	   yum: name=httpd state=present
	 - name: install configure file
	   copy: src=/apps/work/files/httpd.conf dest=/etc/httpd/conf/
	   tags: instconf              #tags
	 - name: start httpd service
	   service: name=httpd state=started
	[root@centos7_1 yaml]# ansible-playbook -tinstconf  web-3.yml 

#指定tags instconf 执行。
	ansible-playbookweb-3.yml --tags=" instconf "  
执行此命令同样仅执行instconf 标签内容。

9、tepmplates 模板的使用

template是文本文件，嵌套有脚本(使用模板编程语言编写)的配置文件。

讲到template模板就不得不先介绍template使用语言 jinja2。
9.1、jinja2语言

    Jinja2是基于python的模板引擎，功能比较类似于PHP的smarty，J2ee的Freemarker和velocity。它能完全支持    unicode，并具有集成的沙箱执行环境，应用广泛。

Jinja2 语言：

	  字面量：
	    字符串：使用单引号或双引号；
	    数字：整数，浮点数
	    列表：[item1,item2 …..]
	    元组：(item1item2…,)
	    字典：{key1：value，key2：value….}
	      布尔型： true/filase
	    算数运算：
	      +,- , * , / , // , % **
	    比较操作：
	      ==， != , >=  ,<=
	    逻辑运算：
	      and，or， not，
	    流表达式
	      For、IF、when

示例：模板安装nginx

模板配置文件nginx.conf.j2

	Worker_porcesses {{ ansible_precossor_vcpus }}  #注意空格哦。
	此变量执行ansible all -m setup  (收集到的远程主机的变量) 即可查看到
	Worker_porcesses {{ ansible_precossor_vcpus +1 }}
	此表达式也可。此处只为表示可支持算数运算。

	[root@centos7_1 yaml]# vim nginx.yml
	---
	- hosts: web
	 remote_user: root
	 tasks:
	 - name: install nginx
	  yum: name=nginx state=present
	 - name: install conf file
	  template: src=/apps/work/files/nginx.conf.j2 dest=/etc/nginx/nginx.conf
	  notify: restart nginx
	  tags: instconf
	 - name: start nginx service
	  service: name=nginx state=started
	 handlers:
	 - name: restart nginx
	    service:name=nginx state=restarted
	[root@centos7_1 yaml]# ansible-playbook  nginx.yml

9.2、when条件判断    

	when 语句：在task中使用。Jinja2的语法格式

	tasks：
	- name: install conf file to Centos7
	  template:src=files/nginxconf.c7.j2 dest=/etc/nginx/nginx.conf
	when: ansible_distribution_major_version==”7”
	- name: install conf file to Centos6
	  template:src=files/nginxconf.c6.j2 dest=/etc/nginx/nginx.conf
	  when:ansible_distribution_major_version ==”6”

	以上语法表示若查询远程主机系统为centos6则执行，install conf file to Centos6。

	若为cenos7则执行install conf file to Centos7。
	
9.3、迭代with_items

    循环迭代，需要重复执行的任务；对迭代项引用，固定变量名为item，而后在task中使用with_items给定迭代的元素列表；

         列表方法：

             字符串

             字典

示例1：

	字符串方式

	- name： install some package
	  yum：name={{ item }}  state=present
	   with_items:
	  - nginx
	   - memecached
	   - php-fpm

示例2：

字典方式

    - name: add  some groups
    group: name={{ item }} state=present
    with_items:
    - group1
    - group2
    - group3
    - name: add some user
       user: name={{ item.name }} group={{item.group}} state=present
      with_items:
      - {name: 'user1',group: 'group1'}
      - {name: 'user2',group: 'group2'}
      - {name: 'user3',group: 'group3'}

10、Playbook执行结果解析

使用ansible-playbook运行playbook文件，得到如下输出信息，输出内容为JSON格式。并且由不同颜色组成，便于识别。一般而言

    绿色代表执行成功，系统保持原样

    黄色代表系统代表系统状态发生改变

    红色代表执行失败，显示错误输出。

11、Playbook案例剖析

实例：
	---
	- hosts: all
	  sudo: yes

	  tasks:
	   - name: 安装Apache
	     yum: name={{ item }} state=present
	     with_items:
	     - httpd
	     - httpd-devel
	   - name: 复制配置文件
	     copy:
	       src=\'#\'" /tmp/httpd.conf",
		 dest: "/etc/httpd/conf/httpd.conf" }
	     - {
	       src=\'#\'" /tmp/httpd-vhosts.conf",
	       dest: "/etc/httpd/conf/httpd-vhosts.conf"
	       }
	   - name: 检查Apache运行状态，并设置开机启动
	     service: name=httpd state=started enabled=yes

12、执行playbook文件

运行playbook，使用ansible-playbook命令

(1) 检测语法

	ansible-playbook  --syntax-check  /path/to/playbook.yaml

(2) 测试运行

	ansible-playbook -C /path/to/playbook.yaml
	--list-hosts   # 列出主机
	--list-tasks  # 列出任务
	--list-tags   # 列出标签

 (3) 运行

	ansible-playbook  /path/to/playbook.yaml
	-t TAGS, --tags=TAGS
	--skip-tags=SKIP_TAGS
	--start-at-task=START_AT

在执行playbook前，可以做些检查

检查palybook语法

	ansible-playbook -i hosts httpd.yml --syntax-check

列出要执行的主机

	ansible-playbook -i hosts httpd.yml --list-hosts

列出要执行的任务

	ansible-playbook -i hosts httpd.yml --list-tasks

13、debug你的playbook

	检查语法：ansible-playbook --syntax-check playbook.yml 
	查看host列表：ansible-playbook --list-hosts playbook.yml
	查看task列表：ansible-playbook --list-tasks playbook.yml
	检查模式(不会运行): ansible-playbook --check playbook.yml
	diff模式(查看文件变化)： ansible-playbook --check --diff playbook.yml
	从指定的task开始运行：ansible-playbook --start-at-task="install packages" playbook.yml
	逐个task运行，运行前需要你确认：ansible-playbook --step playbook.yml
	指定tags：ansible-playbook --tags=foo,bar playbook.yml
	跳过tags：ansible-playbook --skip-tags=baz,quux playbook.yml

六：ROLES

ROLES 角色

    对于以上所有的方式有个弊端就是无法实现复用假设在同时部署Web、db、ha 时或不同服务器组合不同的应用就需要写多个yml文件。很难实现灵活的调用。。
    roles 用于层次性、结构化地组织playbook。roles能够根据层次型结构自动装载变量文件、tasks以及handlers等。要使用roles只需要在playbook中使用include指令即可。简单来讲，roles就是通过分别将变量(vars)、文件(file)、任务(tasks)、模块(modules)及处理器(handlers)放置于单独的目录中，并可以便捷地include它们的一种机制。角色一般用于基于主机构建服务的场景中，但也可以是用于构建守护进程等场景中。
1、目录层级结构

    roles每个角色中，以特定的层级目录进行组织

	Mysql/  角色
	 Files/     #存放有copy或script模块等调用的文件；’
	 Tepmlates/    #template模块查找所需要模板文件目录；
	 Tasks/           #定义任务；至少应该包含一个名为main.yml的文件；其他的文件需要在此文件中通过include进行包含。
	 Handlers/      #定义触发器；至少应该包含一个名为main.yml的文件；其他的文件需要在此文件中通过include进行包含。
	 Vars/              #定义变量；至少应该包含一个名为main.yml的文件；其他的文件需要在此文件中通过include进行包含。
	 Meta/             #定义变量；至少应该包含一个名为main.yml的文件；定义当前角色的特殊设定及其依赖
	关系；其他的文件需要在此文件中通过include进行包含。
	 Default/         #设定默认变量时使用此目录中的main.yml文件。

2、角色调用

	[root@centos7_1 yaml]# vim roles.yml
	   ---
	    Hosts：web
	   Remote_user：root
	   Roles：
	   - mysql
	   - memchached
	   - nginx


3、层级结构展示

示例1：利用ansible角色安装nginx
 

	[root@centos7_1 ~]# mkdir/etc/ansible/roles/nginx/{files,tasks,templates,handlers,vars, \
	default,mata} –pv
	#创建固定目录结构
	[root@centos7_1 ~]# tree  /etc/ansible/roles/nginx/
	/etc/ansible/roles/nginx/
	├── default
	├── files
	├── handlers
	├── mata
	├── tasks
	├── templates
	└── vars
	[root@centos7_1 ~]# cd/etc/ansible/roles/nginx/
	[root@centos7_1 nginx]# vimtasks/main.yml  #创建任务
	- name: install nginx package
	 yum: name=nginx state=present
	- name: install conf file
	 template: src=nginx.conf.j2 dest=/etc/nginx/nginx.conf 
	 #此处源文件可不写绝对路径，系统自查找。
	- name: start nginx
	 service: name=nginx state=started
 
	[root@centos7_1 ~]# cp/apps/work/files/nginx.conf.c6.j2 ../templates/nginx.conf.j2 
	#将配置文件拷贝至templates目录内。
	[root@centos7_1 ~]# cd /apps/yaml/
	[root@centos7_1 yaml]# cat roles.yml #创建调用文件
	---
	- hosts: web
	 remote_user: root
	 roles:
	 - nginx
	[root@centos7_1 yaml]#ansible-playbook roles.yml  #利用ansible-playbook执行。

示例2：变量调用
	利用定义变量使远程主机的nginx服务运行用户变更为daemon

	[root@centos7_1 ~]# vim/etc/ansible/roles/nginx/vars/main.yml
	username: daemon
	[root@centos7_1 ~]# vim/etc/ansible/roles/nginx/templates/nginx.conf.j2
	user {{ username }};  #  将此处原有用户修改为变量
	[root@centos7_1 ~]# cd/apps/yaml/
	[root@centos7_1 yaml]#ansible-playbook  roles.yml
	[root@centos7_1 yaml]#ansible-playbook  -e"username=adm"  roles.yml
	#也可以直接利用命令行传递变量参数给剧本文件。

示例3：在playbook调用角色方法:传递变量给角色

	[root@centos7_1 yaml]vim roles.yml
	---
	 - hosts：web
	  remote_user:root
	  roles:
	  - {role: nigix, username: nginx } 
	  #在调用nginx角色是使用变量username:nginx时服务运行用户为nginx
	   键role:用于指定角色名称；后续的键值对用户传递变量给角色
	[root@centos7_1yaml]# ansible-playbook roles.yml

示例4：条件测试角色调用
   还可以基于条件测试实现角色调用；

	[root@centos7_1yaml]vim roles.yml
	---
	- hosts：web
	  remote_user: root
	  roles:
	 {role: nigix, username: nginx ,when: “ansible_distribution_major_version ==’7’”}
	#基于条件测试调用变量赋予nginx。
	[root@centos7_1 yaml]#ansible-playbook -t instconf  roles.yml

示例5：角色安装

	[root@centos7_1 ~]# mkdir/etc/ansible/roles/memcached/tasks -pv
	[root@centos7_1 ~]# vim  /etc/ansible/roles/memcached/tasks/main.yml
	- name: install package
	 yum: name=memcached state=present
	- name: start memecached
	 service: name=memcached state=started

	[root@centos7_1 ~]# cd/apps/yaml/
	[root@centos7_1 yaml]# cat mem.yml
	---
	- hosts: web
	  remote_user: root
	  roles:
	  - { role: nginx,when:ansible_distribution_version == '7' }  
	  #系统为centos7时调用执行nginx
	  - { role: memcached,when: ansible_hostname =='memcached' }  
	  #系统用户名为memcached的主机调用执行角色memcached。

示例6：角色变量调整memcached内存大小
利用变量使远程主机上的Memcahed的缓存大小占用系统内存大小的三分之一。

	[root@centos7_1 ~]# cd/etc/ansible/roles/memcached/
	[root@centos7_1 memcached]#ls
	handlers/  tasks/    templates/
	[root@centos7_1 memcached]#mkdir  templates
	[root@centos7_1memcached]# scp 172.16.254.216:/etc/sysconfig/memcached \
	    ./templates//memcached.j2
	[root@centos7_1 memcached]#vim templates/memcached.j2
	PORT="11211"
	USER="memcached"
	MAXCONN="1024"
	CACHESIZE="{{ansible_memtotal_mb//3 }}"
	 #变量设置内存的3分之一  此变量为远程主机的总内存//3 指除3取商
	  便为远程主机的三分之一
	[root@centos7_1 memcached]#mkdir handlers/
	[root@centos7_1 memcached]#vim handlers/main.yml
	- name: restart memcached
	  service: name=memcached state=restarted
	[root@centos7_1 memcached]#cd /apps/yaml/
	root@centos7_1 yaml]#ansible-playbook   mem.yml  #执行剧本

4、时间计时模块

ansible中可以加入一个计时模块在执行ansible-playbook时显示执行时长。方便使用。

	1、配置方法
	  cd /etc/ansible
	  mkdir callback_plugins
	  cd callback_plugins
	  wget https://raw.githubusercontent.com/jlafon/ansible- \ profile/master/callback_plugins/profile_tasks.py
	注意：ansible2.0以上版本需在ansible.cdg中加入以下内容
	  [defaults] 下面加入
	  callback_whitelist= profile_tasks
	  再次执行ansbile-playbook时显示执行时长
	2、测试结果

参考文档：

https://blog.csdn.net/liuxiangke0210/article/details/80297488
