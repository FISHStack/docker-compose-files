
# 直接在docker service中使用config

## 在docker swarm种创建配置文件
```
echo port 6379 > redis.conf
docker config create redis.conf redis.conf
docker config ls
```  

## 使用config作为配置文件
```
docker service create --config redis.conf --name myredis redis:5-alpine
```  

## 更新配置文件

> 前提是需要有一个新的配置文件 省略新配置文件生成命令,与上述一致

```
docker service update --config-rm redis.conf --config-add source=redis2.conf,target=redis.conf myredis
```  

> docker service update --args 'redis-server /redis2.conf' containerID  直接改变启动命令也可以,不删除容器中的config,未测试

# 在docker-compose部署的swarm中使用config

```
docker stack deploy -c docker-compose.yml myredis
```

# config在docker中没有更新操作

只有新增和删除,迭代版本只能删除了再新增或者新增一个不一样的config name,可以使用版本号或日期来进行迭代