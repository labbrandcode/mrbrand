FROM ubuntu:18.04
LABEL AlbertShen="Albert Shen <albertshen1206@gmail.com>" \
      Description="Container with Nginx 1.16 & PHP-FPM 7.2 based on Ubuntu Linux."

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
COPY config/apt/sources.list /etc/apt/sources.list

# Install packages
RUN apt-get update && apt-get install -y nginx-full curl php7.2-fpm php7.2-mysql php7.2-gd php7.2-json \
    php7.2-cli php7.2-sqlite3 php-memcached php-redis php7.2-zip php7.2-bz2 php7.2-mbstring php7.2-xml

# Backup nginx
RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
# Configure nginx
COPY config/nginx/nginx.conf /etc/nginx/nginx.conf

# Configure upstream
COPY config/nginx/upstream_phpcgi_tcp.conf /etc/nginx/upstream_phpcgi_tcp.conf

# Backup default server definition
RUN mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak

# Delete symbolic link
RUN rm -rf /etc/nginx/sites-enabled/default


# Copy default server definition
COPY config/nginx/sites-available/www.conf /etc/nginx/sites-available/www.conf

# Create symbolic link for www.conf
RUN ln -s /etc/nginx/sites-available/www.conf /etc/nginx/sites-enabled/www.conf


# Backup php pool
RUN mv /etc/php/7.2/fpm/pool.d/www.conf /etc/php/7.2/fpm/pool.d/www.conf.bak
# Configure php-fpm pool
COPY config/php/fpm/pool.d/www.conf /etc/php/7.2/fpm/pool.d/www.conf

# Restart the php-fpm sevice
# CMD ["nginx", "-g", "daemon off;"]
COPY config/start.sh /root/start.sh

RUN chmod 755 /root/start.sh

WORKDIR /root

CMD ["./start.sh"]
