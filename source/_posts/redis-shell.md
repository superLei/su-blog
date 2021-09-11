---
title: redis 批量操作
categories:
- 技术
tags:
- redis
- shell
---

## redis shell
### 用shell批量执行redis命令
```bash
#!/bin/bash
host=$1
port=$2
password=$3
cat command.txt | /home/redis/redis-cli -h $host -p $port -a $password --pipe

# 然后在redis所在的机器上执行  sh xxx.sh localhost 6379,这样就能快速的执行command.txt中的所有命令
```
command.txt文件中填写要执行的redis命令：
```
select 10
setex MAUTH:SESSION:ma_user:10000040000 604800 "{\"cType\":\"H5\",\"exMap\":{\"groupID\":86167},\"hllAppID\":\"wxe4a96de22f985518\",\"hllMpID\":\"hualala_com\",\"hllOpenID\":\"50001\",\"loginUserBase\":{\"cityName\":\"\u9ed4\u4e1c\u5357\",\"isPerfect\":true,\"nickName\":\"\u95f2\u4eba\",\"provinceName\":\"\u8d35\u5dde\",\"remark\":\"\",\"sex\":0,\"userPhoto\":\"\"},\"mpID\":\"\",\"openAppID\":\"\",\"subAppID\":\"\",\"subMpID\":\"\",\"subOpenID\":\"\",\"unionID\":\"\",\"userID\":50001}"
```

### 在shell下批量管理redis数据
```bash
# 1：批量删除key
redis-cli -p 6393  -n 1 keys *detail_*  |xargs -i redis-cli -p 6393 -n 1 del {} 
# 2：批量移动key，从db 1 移到 db 0
redis-cli -p 6393  -n 1 keys "detail_*"  |xargs -i redis-cli -p 6393 -n 1 move {} 0
```