# hadoop & spark install toolkit
快速安装高可用 Hadoop & Spark 集群，设置基本配置，让大家迅速上手


## 机器
分两类：master, slave

__master 要求：__
* sshd, python 2, fabric
* 3台即可，建议硬件、软件配置完全相同
* 对硬件性能要求不高，一般服务器都可以满足

**slave 要求：**
* sshd
* 建议3~100台，硬件、软件配置完全相同，存储、CPU、内存越大越好，参考 hadoop, spark 对硬件要求


## 快速安装
1. 下载
2. 编辑配置文件：conf/hosts、conf/config
3. 手工下载软件包到 packages 目录
4. 执行一键安装：
```
./install.sh all
```

## 手动安装
1. 下载本项目所有内容到一台 master 机器
2. 编辑配置文件：conf/hosts、conf/config
3. 如果有其它 master 机器，拷贝此目录到其它的 master 机器
4. 去所有的 master 机器，执行如下命令：
`./init-master.sh`
5. 选择一台 master 机器（需要下载安装包），执行如下操作：
```bash
./init-all.sh
```

手工下载软件包到 packages 目录

如果配置了多台 master，则需要安装 zookeeper：
```
./install zookeeper
```

安装其它组件：
```
./install hadoop
./install spark
```

## 启动服务

进入 master 机器的安装目录，比如 /usr/local/myhadoop/
执行 admin.sh 会看到帮助信息
```
> ./admin.sh
Usage: ./admin.sh {zookeeper|hadoop|spark} {start|stop}
```

**先启动 zookeeper**
`./admin.sh zookeeper start`

**再启动，如果是第一次启动 hadoop，需要先初始化一下：**
```
cd hadoop
./namenode_format.sh
```

**启动 hadoop**
`./admin.sh hadoop start`


## TODO 其它辅助
ntp 服务器
Mysql
DNS


## 适用OS
+ Ubuntu 14.04.1 LTS
+ TODO: Centos 6


## WARN
操作不可逆
TODO: 提供删除脚本，但有些操作不是完全可逆，比如配置用户名、hosts，需要保存之前的状态才能做到准确撤销


## 修改的OS配置：
* set hostname of all machines (different by OS)
* /etc/hosts
* create new user and group defined by config: run.user, run.group
* install jdk at /usr/java/, and defion JAVA_HOME at last of /etc/profile
* /root/.ssh, password-less ssh login, from all master to all slave as run.user
* create directory defined by config: basedir.install, basedir.log, basedir.data


