---
authors: wangquan
tags: [learn,note]
---

### docker安装oracle23
<!-- truncate -->
```shell
docker pull container-registry.oracle.com/database/free:latest

# 创建 Docker 卷
docker volume create oracle_data

# 查看Docker卷在本机上的位置
docker volume inspect oracle_data

# 运行 Oracle 容器并挂载卷
docker run -d -it --name oracle23c \
  -p 1521:1521 -p 5500:5500 \
  -e ORACLE_PWD=YourPassword \
  -v oracle_data:/opt/oracle/oradata \
  container-registry.oracle.com/database/express:23c

```

#### 启动oracel

```sql
# 启动容器
docker start oracle23c

# 连接到 FREEPDB1 可插拔数据库
docker exec -it oracle23c sqlplus sys/XMZXFC430724wq@localhost:1521/FREEPDB1 as sysdba

# 查看docker内部oracle所在的文件系统
docker exec -it oracle /bin/bash

# 在 SQL*Plus 中执行以下 SQL 命令
CREATE TABLESPACE my_blog
DATAFILE '/opt/oracle/oradata/XE/my_blog.dbf' 
SIZE 50M
AUTOEXTEND ON
NEXT 10M MAXSIZE 200M
EXTENT MANAGEMENT LOCAL;

CREATE USER my_blog_user IDENTIFIED BY my_blog_password
DEFAULT TABLESPACE my_blog
QUOTA UNLIMITED ON my_blog;

GRANT CONNECT, RESOURCE TO my_blog_user;

# 连接到 FREEPDB1 可插拔数据库
docker exec -it oracle23c sqlplus my_blog_user/my_blog_password@localhost:1521/FREEPDB1

# 在 SQL*Plus 中执行以下命令来验证
SELECT username, default_tablespace FROM dba_users WHERE username = 'MY_BLOG_USER';

select username,default_tablespace from dba_users where username='MYBLOG';

密码：XMZXFC430724wq
```
