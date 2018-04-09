# LCG - Linux Configuration Guide - MongoDB
# MongoDB安装配置指引
### 安装
* 安装MongoDB

### 更改默认数据及日志路径  
* 创建数据及日志路径  
`mkdir /data/mongodb/data`
`mkdir /data/mongodb/log`
`sudo ln -s /data/mongodb/data/ /var/lib/mongo`
`sudo ln -s /data/mongodb/log/ /var/log/mongodb`
`chown mongod:mongod /data/mongodb/`
`chown mongod:mongod /data/mongodb/data`
`chown mongod:mongod /data/mongodb/log`

* 打开`/etc/mongod.conf`并编辑如下内容:  
日志路径  
```
systemLog:
  path: /data/mongodb/log/mongod.log
```

db路径  
```
storage:
    dbPath: /data/mongodb/data
```

### Auth  
* 打开`/etc/mongod.conf`并编辑如下内容:  
安全设置  
```
security:
  authorization: enabled
```

* 创建数据库管理员   
`mongo`
`use admin`
`db.createUser({user: "root", pwd: "password", roles:[{role: "userAdminAnyDatabase", db: "admin"}]})`
`quit()`
校验用户  
`mongo -u root -p --authenticationDatabase admin`

* 创建应用数据用户  
`use app_db`
`db.createUser({user: "app_user", pwd: "password", roles:[{role:"readWrite", db: "app_db"}]})`
`quit()`
校验用户  
`mongo -u app_user -p --authenticationDatabase app_db`

### 性能
增加用户限制  
`echo "mongod     soft    nofiles   64000" >> /etc/security/limits.conf`
`echo "mongod     soft    nproc     64000" >> /etc/security/limits.conf`
