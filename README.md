# Signal-Server-deploy
The steps and notice info when deploy Signal Server on your own Server.


## Overview
  - based on [io.dropwizard](https://www.dropwizard.io/1.3.5/docs/) API framework;
  - used pom to manage and compile;
  - only compatible with PostgreSql DB so far, imcompatible with MySql, there are some syntax errors with MySql, especially JOSN type;
  - used Redis server for caching;
  - used twilio for SMS, turn for NAT detection, AWS S3 for file attachment and profile avator, but you need to change with your local provider.

## Server environment
  Server Machine Info: CentOS 6.6, YUM for install components.
  ### Redis config
  1. Install Redis with: <br />
  ```yum install redis``` <br />
  2. You will get: <br />
  ```bash
  ll /usr/bin | grep redis
  redis-server
  redis-cli
  ...
  ``` 
  Redis config file located: ```/etc/redis.conf``` <br />
  3. Config Redis to **Master-Slave Replication**, you can search the key words on internet. 
  The main config on slave instance is:
  ```slaveof 127.0.0.1 6379``` <br />
  meanwhile, you need rename the pid / logfile / port / dbfilename for each instance of Redis server.<br />
  4. Then start each Redis server:
  ```bash
  redis-server /etc/redis.conf
  redis-server /etc/redis_slave_6380.conf
  redis-server /etc/redis_slave_6381.conf
  ```
  The master instance is for write, and the others is for read.
  
  ### PostgreSql Db
  1. Postgresql install refer to [Postgrsql download binary](https://www.postgresql.org/download/); <br />
  2. After install, init Postgresql and changed to auto run: <br />
  ```bash
  service postgresql-10 initdb
  chkconfig postgresql-10 on
  service postgresql-10 start
  ``` 
  3. Create user and database 
  ```bash
  su postgres
  bash-4.1$ psql
  psql (10.5)
  Type "help" for help.
  
  postgres=#
  postgres=#\password postgres ##change the postgres user passoword
  
  postgres=#CREATE USER sig_user WITH PASSWORD 'Sig123';
  postgres=#CREATE DATABASE accountdb OWNER sig_user;
  postgres=#CREATE DATABASE messagedb OWNER sig_user;
  ```
  4. Change pg_hba.conf: 
  ```bash
  vi /var/lib/pgsql/10/data/pg_hba.conf
  
  # TYPE  DATABASE        USER            ADDRESS                 METHOD
  
  # "local" is for Unix domain socket connections only
  local   all             all                                     md5
  # IPv4 local connections:
  host    all             all             127.0.0.1/32            md5
  # IPv6 local connections:
  host    all             all             ::1/128                 md5
  # Allow replication connections from localhost, by a user with the
  # replication privilege.
  local   replication     all                                     md5
  host    replication     all             127.0.0.1/32            md5
  host    replication     all             ::1/128                 md5
  ```
  change all the METHOD to **md5** <br />
  5. Then ```service postgres-10 restart```, restart PostgreSql service.
  
  
  
  
  
  
## References:
1. [skal1ozz/RedPhoneServer](https://github.com/skal1ozz/RedPhoneServer)
