## 安装RocketMQ

```shell script
# rocketmq
docker pull rocketmqinc/rocketmq:4.4.0

docker run -d -p 9876:9876 \
-v /data/wanxin/rocketmq/namesrv/logs:/root/logs \
-v /data/wanxin/rocketmq/namesrv/store:/root/store \
--name rmqnamesrv \
-e "MAX_POSSIBLE_HEAP=100000000" rocketmqinc/rocketmq:4.4.0 sh mqnamesrv

docker run -d -p 10911:10911 -p 10909:10909 \
--privileged=true \
-v /data/wanxin/rocketmq/broker/logs:/root/logs \
-v /data/wanxin/rocketmq/broker/store:/root/store \
-v /data/wanxin/rocketmq/conf/broker.conf:/opt/rocketmq-4.4.0/conf/broker.conf \
--name rmqbroker \
--link rmqnamesrv:namesrv \
-e "NAMESRV_ADDR=namesrv:9876" \
-e "MAX_POSSIBLE_HEAP=200000000" \
rocketmqinc/rocketmq:4.4.0 sh mqbroker -c /opt/rocketmq-4.4.0/conf/broker.conf
```

## 监控

```shell script
# 监控
docker pull styletang/rocketmq-console-ng

docker run -d --name rmqconsole \
-p 18080:8080 \
-e "JAVA_OPTS=-Drocketmq.namesrv.addr=192.168.3.35:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" \
styletang/rocketmq-console-ng

systemctl stop firewalld.service
```
