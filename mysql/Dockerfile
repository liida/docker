FROM mysql:5.7.33
COPY ./conf/my.cnf /etc/mysql/my.cnf
#设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

CMD [ "mysqld"]