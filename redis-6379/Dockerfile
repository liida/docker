FROM redis:6.0.0
COPY ./conf/redis.conf /etc/redis/redis.conf
#设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

CMD [ "redis-server", "/etc/redis/redis.conf" ]