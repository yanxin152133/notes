# 1. Mongodb

## 1.1. mongod.conf
```conf
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /data/log/mongod.log

# Where and how to store data.
storage:
  dbPath: /data/db

# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.


security:
  authorization: enabled

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options

#auditLog:
```

## 1.2. 自定义配置
运行以下命令：

```bash
mkdir /home/ubuntu/mongodb/conf
mkdir /home/ubuntu/mongodb/logs
mkdir /home/ubuntu/mongodb/data
touch /home/ubuntu/mongodb/logs/mongod.log
chmod 777 /home/ubuntu/mongodb/logs/mongod.log
```

## 1.3. docker
```bash
docker run -d --restart=always --name mongodb \
    -p 27017:27017 \
    -v /home/ubuntu/mongodb/data:/data/db \
    -v /home/ubuntu/mongodb/conf:/data/configdb \
    -v /home/ubuntu/mongodb/logs:/data/log/  \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=yan1+ \
    -d \
    mongo:latest \
    -f /data/configdb/mongod.conf
```