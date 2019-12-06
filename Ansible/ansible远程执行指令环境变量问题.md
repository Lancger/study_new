```bash
环境变量需要加到

* 我们登录执行的是login shell，会加载/etc/profile,~/.bash_profile
* ansible这类ssh远程执行是non-login shell，不会加载etc/profile,~/.bash_profile,而是加载etc/bashrc和~/.bashrc


* login shell加载环境变量的顺序是：① /etc/profile ② ~/.bash_profile ③ ~/.bashrc ④ /etc/bashrc
* 而non-login shell加载环境变量的顺序是： ① ~/.bashrc ② /etc/bashrc


结论

ansible这类远程执行的non-login shell 并不会加载/etc/profile和~/.bash_profile下的环境变量，只是加载~/.bashrc和/etc/bashrc
如果需要在ansible中执行需要特定环境变量的命令，可以在执行前source一下~/.bash_profile， 或者将环境变量写在~/.bashrc 。
```
参考资料：

https://blog.csdn.net/u010871982/article/details/78525367 关于ansible远程执行的环境变量问题（login shell & nonlogin shelll）
