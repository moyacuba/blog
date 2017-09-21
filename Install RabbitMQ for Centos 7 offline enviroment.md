
##Install & Configure RabbitMQ in Centos 7 offline mode

### Steps once you copied all required binaries

1- Install rabbitmq-release-signing-key
```shell
rpm --import rabbitmq-release-signing-key.asc.txt
```

2- Install ErLang
```shell
yum install erlang-20.0.5-1.el7.centos.x86_64.rpm
```

3- Install SOCAT RabbitMQ's dependency
```shell
yum install socat-1.7.3.2-2.el7.x86_64.rpm
```

4- Install RabbitMQ RPM
```shell
yum install rabbitmq-server-3.6.12-1.el7.noarch.rpm
```

5- Fix SELinux and the firewall

Read this [article RabbtiMQ Port Access](http://www.rabbitmq.com/networking.html#selinux-ports) documentation for details.
```shell
firewall-cmd --permanent --add-port=4369/tcp
firewall-cmd --permanent --add-port=5672/tcp
firewall-cmd --permanent --add-port=5671/tcp
firewall-cmd --permanent --add-port=25672/tcp
firewall-cmd --permanent --add-port=15672/tcp
firewall-cmd --permanent --add-port=61613/tcp
firewall-cmd --permanent --add-port=61614/tcp
firewall-cmd --permanent --add-port=1883/tcp
firewall-cmd --permanent --add-port=8883/tcp
firewall-cmd --permanent --add-port=15674/tcp
firewall-cmd --permanent --add-port=15675/tcp
firewall-cmd --reload
setsebool -P nis_enabled 1
```
