# Base images 基础镜像
FROM centos:centos7
#MAINTAINER 维护者信息
MAINTAINER liida 286503439@qq.com
#添加www用户
RUN useradd www -u 1200 -M -s /sbin/nologin
#设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
#安装第三方源epel源
RUN yum install -y epel-release \
    http://rpms.remirepo.net/enterprise/remi-release-7.rpm  \
    yum-utils && \
    yum-config-manager --enable remi-php73 && \
    yum install -y \
        php \
        php-devel \
        php-fpm \
        php-bcmath \
        php-gd \
        hp73-php-imap \
        php-ldap \
        php-mcrypt \
        php-mbstring \
        php-mysqlnd \
        php-opcache \
        php-pdo \
        php-pgsql \
        php-soap \
        php-tidy \
        php-json \
        php-xml \
        php-pecl-apcu \
        php-pecl-apcu-bc \
        php-pecl-geoip \
        php-pecl-igbinary \
        php-pecl-imagick \
        php-pecl-memcached \
        php-pecl-redis \
        php-pecl-mongodb \
        php-pecl-swoole \
        php-pecl-protobuf \
        php-pecl-yaf \
        php-pecl-msgpack \
        php-pecl-yaml \
        php-pecl-zip && \
    yum --enablerepo=epel install -y fcgi && \
    yum clean all
#拷贝文件
COPY ./etc/ /etc/
#生成目录
RUN mkdir -p /run/php-fpm
RUN chmod 777 -R /var/lib/php
#切换目录
WORKDIR  /

EXPOSE 9000
#CMD ["/bin/sh"]
CMD ["/usr/sbin/php-fpm","-F", "-R"]