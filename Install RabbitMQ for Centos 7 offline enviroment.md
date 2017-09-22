
# Install & Configure RabbitMQ in Centos 7 offline mode

## Steps once you copied all required binaries

1- Install the ASCII file downloaded from (https://www.rabbitmq.com/rabbitmq-release-signing-key.asc)
```shell
rpm --import rabbitmq-release-signing-key.asc
```

2- Install ErLang
```shell
yum install erlang-20.0.5-1.el7.centos.x86_64.rpm
```

3- Install SOCAT RabbitMQ Server's dependency
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

6- Setup enviroment & configuration files:

6.1 You can change some parameters using enviroment variables, check documentation for details at (http://www.rabbitmq.com/configure.html#define-environment-variables)[http://www.rabbitmq.com/configure.html#define-environment-variables]

6.2 Or you can use the file ```rabbitmq-env.conf```, by default in Centos rabbitmqserver read it from folder ```/etc/rabbitmq/``` unless you use enviroment variable to change its path; the RabbitMQ Server will append ```.conf``` extension .
An example for ```rabbitmq-env.conf```:
```txt
# ##############################
# RABBITMQ SERVER CONFIGURATION
#
# All of the parameters below can also
# be set as environment variables
# using the prefix "RABBIT_"
# i.e. export RABBITMQ_NODE_PORT=5672
# ##############################

# ##########################
# Defaults to the empty string
# meaning bind to all network interfaces
# Change to bind to one network interface only
# ##########################
# NODE_IP_ADDRESS=
# NODE_PORT=5672

# ##########################
# Mac/Unix: rabbit@$HOSTNAME
# The node name should be unique per erlang-node-and-machine combination
# To run multiple nodes, see the clustering guide
# ##########################
NODENAME=testInVM

# ##########################
# Unix/deb: /etc/rabbitmq/rabbitmq
# If the configuration file is present it is used by the server
# The .config extension is automatically appended
# This file is also used to auto-configure RabbitMQ clusters
# Example:
# Config file location and new filename bunnies.config
# CONFIG_FILE=/etc/rabbitmq/bunnies
# ##########################
CONFIG_FILE=/etc/rabbitmq/rabbitmq
```