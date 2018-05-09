FROM centos:7
MAINTAINER mail@huangming.org
RUN yum install -y gcc gcc-c++ make gd-devel zlib-devel libtool libxslt-devel libpng-devel bzip2-devel libXpm-devel libxml2-devel libcurl-devel libjpeg libjpeg-devel openssl-devel net-snmp-devel freetype freetype-devel autoconf expat libxslt
ADD php-5.6.35.tar.gz /tmp/

RUN groupadd php-fpm
RUN useradd -g php-fpm -s /sbin/nologin php-fpm -M

RUN cd /tmp/php-5.6.35 && \
    ./configure --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php/etc \
    --enable-fpm \
    --with-fpm-user=php-fpm \
    --with-fpm-group=php-fpm \
    --with-mysql --with-mysqli \
    --with-openssl --with-zlib \
    --with-bz2 --with-curl --with-gd \
    --with-jpeg-dir --with-png-dir --with-iconv \
    --with-pcre-dir --with-libxml-dir --with-freetype-dir \
    --with-mhash --with-pear --with-snmp --with-gettext \
    --enable-zip --enable-mbstring --enable-exif --enable-fpm \
    --enable-bcmath --enable-sockets --enable-ftp \
    --enable-sysvsem --enable-gd-native-ttf --enable-soap && \
    make -j 4 && \
    make install && \
    cp php.ini-production /usr/local/php/etc/php.ini && \
    sed -i "s#;date.timezone =#date.timezone = Asia/Shanghai#" /usr/local/php/etc/php.ini
COPY php-fpm.conf /usr/local/php/etc/

RUN rm -rf /tmp/php-5.6.35* && yum clean all

WORKDIR /usr/local/php
EXPOSE 9000
CMD ["./sbin/php-fpm", "-c", "/usr/local/php/etc/php-fpm.conf"]
