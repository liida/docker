# **nginx1.19+php7.3+mysql5.7+redis6+consul**

**nginx添加lua支持**

**PHP已经安装常用扩展redis,memcached,swoole**



**查询所有的容器，过滤出Exited状态的容器，列出容器ID，删除这些容器**

``docker rm `docker ps -a|grep Exited|awk '{print $1}'`     ``

**启动**
``docker-compose up -d``