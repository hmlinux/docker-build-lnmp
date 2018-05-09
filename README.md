# Dockerfile构建镜像并启动LNMP网站平台#2

构建Nginx镜像(nginx-1.14.0)
    
    # docker build -t reg.huangming.org/lnmp/nginx:1.14.0 .

上传Nginx镜像到私有镜像仓库(Harbor)
    
    # docker login -u hm -p Hm123456 reg.huangming.org
    # docker push reg.huangming.org/lnmp/nginx:1.14.0
 
 
构建PHP镜像(php-5.6.35)
    
    # docker build -t reg.huangming.org/lnmp/php:5.6.35

上传PHP镜像到私有镜像仓库(Harbor)
    
    # docker login -u hm -p Hm123456 reg.huangming.org
    # docker push reg.huangming.org/lnmp/php:5.6.35


创建网络
    
    # docker network create lnmp


创建站点目录
    
    # tar -zxf wordpress-4.9.5.tar.gz -C /data/wwwroot/
    # chown -R 1000:1000 /data/wwwroot/wordpress


启动MySQL容器
    
    # docker run -d -it \
    --name lnmp_mysql \
    --net lnmp \
    -p 172.16.10.14:3306:3306 \
    --mount type=bind,src=/data/containers/mysql,dst=/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=123456 \
    mysql:5.6 --character-set-server=utf8


启动PHP容器
    
    # docker run -itd \
    --name lnmp_php \
    --net lnmp \
    --mount type=bind,src=/data/wwwroot,dst=/usr/local/nginx/html \
    reg.huangming.org/lnmp/php:5.6.35


启动Nginx容器
    
    # docker run -itd \
    --name lnmp_nginx \
    --net lnmp \
    -p 0.0.0.0:80:80 \
    -v /data/wwwroot:/usr/local/nginx/html \
    -v $(pwd)/conf.d:/usr/local/nginx/conf/conf.d \
    reg.huangming.org/lnmp/nginx:1.14.0


创建数据库
    
    # docker exec lnmp_mysql sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD" -e "create database wordpress"'


Web URL
    
    http://172.16.10.14/wordpress
