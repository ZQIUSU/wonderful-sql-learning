# 环境搭建

## 1.安装mysql
### 这里我直接使用docker pull 一个mysql的镜像

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/ab3cd85a-b729-4fb8-860c-ceb91d97dfc6)

### 以命令行模式启动一个容器

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/ade960ec-0470-4cad-8a46-2e4fd331b802)

-i 表示交互式操作 -t 表示终端

/bin/bash 表示启动一个交互性shell

exit 直接退出容器

docker ps -a 查看所有容器

docker start [container_id] 启动停止的容器

相等的docker stop [container_id] 停止启用容器

### 大部分情况我们希望后台运行容器

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/40200e39-0518-4057-b46d-c262870d671c)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/17a87251-acf7-4ad0-9848-3f60c9259fe1)

### 从后台进入容器

有两个方法，一个是attach，一个是exec

docker attach 退出会导致容器的终止，所以在这里我们演示exec

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/a71d3fb4-64c8-4537-87f4-5f53fb99c8b3)


下边演示的是docker attach

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/195444b5-f4b9-42f0-91db-44e028249131)

## 2.连接mysql和dbeaver

### 先进入一个容器，连接到本地服务器

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/7fe81fff-b3b6-46ee-bfb9-16b609f565ff)

-h localhost 表示连接到本地

-u root 表示使用root访问，root具有超级权限

-p 输入密码

### 连接到服务器后登录dbeaver连接mysql服务器

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/7316c2d7-4f5f-4aa0-a343-92b8b2f1d339)

至此环境搭建完成
