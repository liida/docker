# Base images 基础镜像
FROM centos:centos7

#安装相关依赖
RUN yum -y update && yum -y install  gcc gcc-c++ autoconf automake make && yum -y install  zlib zlib-devel openssl* pcre* wget lua-devel

#MAINTAINER 维护者信息
MAINTAINER liida 286503439@qq.com
#添加www用户
RUN useradd www -u 1200 -M -s /sbin/nologin
#设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
#ADD  获取url中的文件,放在当前目录下
ADD http://nginx.org/download/nginx-1.9.9.tar.gz  /tmp/soft/
#echo-nginx-module 模块 是一个 Nginx 模块，提供直接在 Nginx 配置使用包括 "echo", "sleep", "time" 等指令
ADD https://github.com/openresty/echo-nginx-module/archive/v0.62.tar.gz /tmp/soft/
#LuaJIT
ADD https://github.com/openresty/luajit2/archive/v2.1-20201229.tar.gz /tmp/soft/
#ngx_devel_kit（NDK）模块
ADD https://github.com/vision5/ngx_devel_kit/archive/v0.3.1.tar.gz /tmp/soft/
#lua-nginx-module 模块
ADD https://github.com/openresty/lua-nginx-module/archive/v0.10.14.tar.gz /tmp/soft/
#redis2-nginx-module 模块 是一个支持 Redis 2.0 协议的 Nginx upstream 模块，它可以让 Nginx 以非阻塞方式直接防问远方的 Redis 服务，同时支持 TCP 协议和 Unix Domain Socket 模式，并且可以启用强大的 Redis 连接池功能。
ADD https://github.com/openresty/redis2-nginx-module/archive/v0.15.tar.gz /tmp/soft/
#lua-resty-redis  模块 Lua redis client driver for the ngx_lua based on the cosocket API
ADD https://github.com/openresty/lua-resty-redis/archive/v0.29.tar.gz /tmp/soft/
#set-misc-nginx-module 模块 是标准的HttpRewriteModule指令的扩展，提供更多的功能，如URI转义与非转义、JSON引述，Hexadecimal、MD5、SHA1、Base32、Base64编码与解码、随机数等等
ADD https://github.com/openresty/set-misc-nginx-module/archive/v0.32.tar.gz /tmp/soft/
#nginx ngx_cache_purge模块
ADD http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz  /tmp/soft/
#切换目录
WORKDIR  /tmp/soft

#安装LuaJIT 2.1
RUN tar zxf v2.1-20201229.tar.gz
WORKDIR  /tmp/soft/luajit2-2.1-20201229
RUN make PREFIX=/usr/local/luajit && make install PREFIX=/usr/local/luajit

#切换目录
WORKDIR  /tmp/soft
#安装lua-resty-redis
RUN tar -xzvf v0.29.tar.gz
WORKDIR  /tmp/soft/lua-resty-redis-0.29
RUN make && make install

WORKDIR  /tmp/soft
#安装echo-nginx-module
RUN tar -xzvf v0.62.tar.gz && cp -r echo-nginx-module-0.62/ /usr/local/src/
#安装ngx_devel_kit(NDK)
RUN tar -xzvf v0.3.1.tar.gz && cp -r ngx_devel_kit-0.3.1/ /usr/local/src/
#安装lua-nginx-module模块
RUN tar -xzvf v0.10.14.tar.gz && cp -r lua-nginx-module-0.10.14/ /usr/local/src/
#安装redis2-nginx-module模块
RUN tar -xzvf v0.15.tar.gz && cp -r redis2-nginx-module-0.15/ /usr/local/src/
#安装set-misc-nginx-module模块
RUN tar -xzvf v0.32.tar.gz && cp -r set-misc-nginx-module-0.32/ /usr/local/src/


#安装nginx ngx_cache_purge模块
RUN tar -xzvf ngx_cache_purge-2.3.tar.gz && cp -r ngx_cache_purge-2.3/ /usr/local/src/

RUN mkdir -p {/usr/local/nginx/logs,/var/lock,/usr/local/nginx/conf/lua,,/usr/local/nginx/conf/vhost}
#编译安装Nginx
RUN tar -zxvf nginx-1.9.9.tar.gz
RUN mkdir -p /usr/local/nginx
#设置环境变量
RUN export LUAJIT_LIB=/usr/local/luajit/lib
RUN export LUAJIT_INC=/usr/local/luajit/include/luajit-2.1
RUN sed -i "1i\LUAJIT_LIB=/usr/local/luajit/lib" /usr/local/src/lua-nginx-module-0.10.14/config
RUN sed -i "1i\LUAJIT_INC=/usr/local/luajit/include/luajit-2.1" /usr/local/src/lua-nginx-module-0.10.14/config
RUN cd /tmp/soft/nginx-1.9.9 \
	&& ./configure --prefix=/usr/local/nginx --user=nginx --group=nginx \
	--error-log-path=/usr/local/nginx/logs/error.log \
	--http-log-path=/usr/local/nginx/logs/access.log \
	--pid-path=/usr/local/nginx/logs/nginx.pid \
	--lock-path=/var/lock/nginx.lock \
	--with-ld-opt=-Wl,-rpath,/usr/local/luajit/lib \
	--with-http_stub_status_module \
	--with-http_ssl_module \
	--with-http_sub_module \
	--with-http_realip_module \
	--add-module=/usr/local/src/echo-nginx-module-0.62 \
	--add-module=/usr/local/src/lua-nginx-module-0.10.14 \
	--add-module=/usr/local/src/ngx_devel_kit-0.3.1 \
	--add-module=/usr/local/src/redis2-nginx-module-0.15 \
	--add-module=/usr/local/src/set-misc-nginx-module-0.32 \
	--add-module=/usr/local/src/ngx_cache_purge-2.3 \
	&& make && make install
#参数说明
#--prefix 用于指定nginx编译后的安装目录
#--add-module 为添加的第三方模块，此次添加了fdfs的nginx模块
#--with..._module 表示启用的nginx模块，如此处启用了http_ssl_module模块
#拷贝文件
COPY ./conf/ /usr/local/nginx/conf/
COPY ./lua/ /usr/local/nginx/conf/lua/

RUN ln -s /usr/local/nginx/sbin/* /usr/local/sbin/
#清理
RUN rm -rf /tmp/soft && yum clean all

#切换目录
WORKDIR  /

#EXPOSE 映射端口
EXPOSE 80 443

#CMD 运行以下命令
#CMD ["nginx"]
CMD ["/usr/local/nginx/sbin/nginx","-g","daemon off;"]