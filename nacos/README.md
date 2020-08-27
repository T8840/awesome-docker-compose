### 参考来源：https://github.com/nacos-group/nacos-docker

## 方式一：已有mysql，仅安装nacos  

#### 步骤1：拉取代码
git clone https://github.com/nacos-group/nacos-docker.git  
cd nacos-docker  

#### 步骤2：修改mysql配置
```
## \nacos-docker\env\nacos-standlone-mysql.env
PREFER_HOST_MODE=hostname
MODE=standalone
SPRING_DATASOURCE_PLATFORM=mysql
MYSQL_SERVICE_HOST=127.0.0.1
MYSQL_SERVICE_DB_NAME=nacos
MYSQL_SERVICE_PORT=3306
MYSQL_SERVICE_USER=root
MYSQL_SERVICE_PASSWORD=123456
```

#### 步骤3：初始化nacos数据库

https://github.com/alibaba/nacos/blob/master/config/src/main/resources/META-INF/nacos-db.sql

#### 步骤4：修改standalone-mysql.yaml  
```
version: "2"
services:
  nacos:
    image: nacos/nacos-server:latest
    container_name: nacos-standalone-mysql
    env_file:
      - ../env/nacos-standlone-mysql.env
    volumes:
      - ./standalone-logs/:/home/nacos/logs
      - ./init.d/custom.properties:/home/nacos/init.d/custom.properties
    ports:
      - "8848:8848"
      - "9555:9555"
    restart: on-failure
```

#### 步骤5：执行命令  
docker-compose -f example/standalone-mysql.yaml up

