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
  Redis config file located: ```/etc/redis.conf```
  3. Config Redis to ##Master-Slave Replication## 
  
  
  
  1. Clone the code [Signal-Server](https://github.com/signalapp/Signal-Server) repo to local machine and open with IDE (like Idea IntellJ);
  2. 
  
  
  
  
  
## References:
1. [skal1ozz/RedPhoneServer](https://github.com/skal1ozz/RedPhoneServer)
