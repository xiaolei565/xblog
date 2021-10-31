#### 写在前面:尽量使用压缩包安装，省去复杂步骤
软件地址：[https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.19-winx64.zip)
如果缺vcredist_x64.dll，请搜索下载`Visual C++ Redistributable for Visual Studio 2013/2015`

### 安装步骤

1. 将zip压缩包放到自己的文件夹下，并解压（如：D:\Environment\mysql5.7.19）
1. 将绝对地址，上一步中括号中的地址，加入环境变量
1. 编辑my.ini文件
```shell
[mysqld]
basedir=D:\Environment\mysql5.7.31\
datadir=D:\Environment\mysql5.7.31\data\
port=3306
skip-grant-tables
```

4. 管理员启动CMD，并切换至mysql安装目录下的bin目录，输入 `mysqld -install`，用来安装mysql
4. 再输入`mysqld --initialize-insecure --user=mysql`用来初始化数据文件
4. 此时启动mysql `net start mysql` ，使用无密码连接命令进行连接`mysql –u root –p`
4. 进入mysql命令行后，更改root密码
```shell
update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
```

8. 刷新权限 `flush privileges;`
8. 注释my,.ini文件最后一句`skip-grant-tables`
8. 重启mysql
```shell
net stop mysql
net start mysql
```
