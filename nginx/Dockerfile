FROM centos:7
MAINTAINER mail@huangming.org
RUN yum install -y gcc gcc-c++ make zlib zlib-devel pcre pcre-devel openssl-devel gd gd-devel perl-devel perl-ExtUtils-Embed
ADD nginx-1.14.0.tar.gz /tmp

RUN groupadd nginx
RUN useradd -g nginx -s /sbin/nologin nginx -M
RUN cd /tmp/nginx-1.14.0 && \
    ./configure --prefix=/usr/local/nginx \
    --user=nginx \
    --group=nginx \
    --prefix=/usr/local/nginx \
    --with-http_stub_status_module \
    --with-pcre \
    --with-http_ssl_module \
    --with-mail_ssl_module \
    --with-http_gzip_static_module \
    --with-http_degradation_module \
    --with-http_realip_module \
    --with-http_addition_module \
    --with-http_sub_module \
    --with-http_perl_module \
    --with-stream \
    --with-http_image_filter_module \
    --with-http_flv_module && \
    make -j 2 && \
    make install && \
    mkdir /usr/local/nginx/conf/conf.d

RUN rm -rf /tmp/nginx-1.14.0* && yum clean all

COPY nginx.conf /usr/local/nginx/conf

WORKDIR /usr/local/nginx
EXPOSE 80
CMD ["./sbin/nginx", "-g", "daemon off;"]
