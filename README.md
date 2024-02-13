# EwoMail for docker

安装：
```
sh ./start.sh
```
安装过程有点慢且没有日志，请你直接访问 http://IP:8000 时不时刷新几次，如果超过半小时无法访问，说明安装存在问题。

如果你想要自定义 EwoEmail 的域名，请修改 start.sh 里面的内容。
```
docker exec -d --privileged ewomail /bin/sh -c 'cd /root/install/ && sh start.sh xxx.com';
docker exec -d --privileged ewomail /bin/sh -c "echo '127.0.0.1   mail.xxx.com smtp.xxx.com imap.xxx.com' >> /etc/hosts";
```
将这里的 `xxx.com` 修改成你的域名。


# 降低内存占用
安装完成EwoMail后，可关闭邮件杀毒软件可以降低内存占用，对于运行内存2G以下的服务器可关闭杀毒来降低内存占用，关闭后能大大的降低内存的占用，不影响防垃圾邮件检测。

**查看内存占比命令**
```
free -m
```

centos

**关闭邮件杀软**

**安装vim**
```
yum install vim -y
```

**修改文件（修改前请备份文件）**
```
vim /etc/amavisd/amavisd.conf
```


**在文件尾部加上该行参数**
```
@bypass_virus_checks_maps = (1);
```


**最后按下esc键，输入:wq保存**
**修改文件（参考上面的例子操作命令修改）**
```
vim /usr/lib/systemd/system/amavisd.service
```
在 Wants=clamd@amavisd.service 前面加上#符号*

**保存文件**
修改后
![image](https://github.com/GgBoom-993/EwoMailForDocker/assets/55505202/952fbd2e-c252-4f6a-a62a-3f6b35d45701)

输入以下命令即可完成杀毒软件的关闭
```
systemctl daemon-reload
systemctl stop clamd@amavisd
systemctl disable clamd@amavisd
systemctl restart amavisd
```


