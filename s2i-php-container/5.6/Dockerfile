FROM centos/s2i-base-centos7


MAINTAINER Umar Mubeen
EXPOSE 8000
ENV PHP_VERSION=5.6 \
PATH=$PATH:/opt/rh/rh-php56/root/usr/bin
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm


RUN curl 'https://setup.ius.io/' -o setup-ius.sh && \
bash setup-ius.sh
RUN yum install -y --setopt=tsflags=nodocs --enablerepo=centosplus \
php56-fpm nginx  
RUN yum -y install php56w \
php-adodb \
php-pecl-pdflib-4.1.1 \
php56w-pecl-redis-3.1.0 \
php56w-opcache \
php56w-common \ 
php56w-json \
php56w-dba \
php56w-fpm \
php56w-cli \
nginx \
php56w-devel \
php56w-gd \
php56w-mbstring \
php56w-ldap \
php56w-odbc \
php56w-intl \
php56w-pdo \
php56w-phpdbg \
php56w-pear \
php56w-xml \
php56w- soap \
php56w-fpm-nginx \
libaio

RUN yum clean all -y
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

COPY ./contrib/ /opt/app-root
RUN cp /opt/app-root/etc/conf.d/php-fpm/pool.conf /etc/php-fpm.d/www.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/fpm.conf /etc/php-fpm.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/pools/rosi.conf /etc/php-fpm.d/rosi.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/pools/rosi-webservice.conf /etc/php-fpm.d/rosi-webservice.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/pools/cacti.conf /etc/php-fpm.d/cacti.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/pools/lm.conf /etc/php-fpm.d/lm.conf


RUN cp /opt/app-root/etc/conf.d/php-fpm/php.ini /etc/php.d/php.ini
RUN cp /opt/app-root/etc/conf.d/php-fpm/php-batch.ini /etc/php.d/php-batch.ini
RUN cp /opt/app-root/etc/conf.d/php/oci8.ini /etc/php.d/30-oci8.ini
RUN mkdir -p /opt/app-root/src/appl/opkg/lib/php/extensions/no-debug-non-zts-20131226
RUN mkdir /tmp/sessions && \
mkdir -p /var/lib/nginx && \
mkdir -p /var/log/nginx && \
touch /var/log/nginx/error.log && \
touch /var/log/nginx/access.log && \
chown -R 1001:0 /var/log/nginx && \
chown -R 1001:0 /var/lib/nginx && \
chown -R 1001:0 /opt/app-root /tmp/sessions && \
chmod -R a+rwx /tmp/sessions && \
chmod -R a+rwx /var/log/nginx && \
chmod -R a+rwx /var/lib/nginx && \
chmod -R ug+rwx /var/log/nginx && \
chmod -R ug+rwx /var/lib/nginx && \
chmod -R ug+rwx /opt/app-root && \
#chmod -R a+rwx /opt/app-root/services.sh \
chmod -R a+rwx /opt/app-root && \
chmod -R a+rwx /opt/app-root/services.sh

#Install saprfc       
RUN mkdir -p /etc/sap
RUN chmod -R a+rwx /etc/sap
RUN cp /opt/app-root/etc/conf.d/oracle/saprfc-1.4.1.tar.gz /etc/sap/saprfc-1.4.1.tar.gz
RUN tar -xvzf /etc/sap/saprfc-1.4.1.tar.gz -C /etc/sap && $PATH:/opt/rh/rh-php56/root/usr/bin/phpize && ./configure \
    && make && make install \
    && echo 'extension=saprfc.so' > /etc/php.d/php.ini


# Oracle instantclient

RUN cp /opt/app-root/etc/conf.d/oracle/instantclient-basic-linux.x64-11.2.0.4.0.zip /usr/local/instantclient-basic-linux.x64-11.2.0.4.0.zip
RUN cp /opt/app-root/etc/conf.d/oracle/instantclient-sdk-linux.x64-11.2.0.4.0.zip /usr/local/instantclient-sdk-linux.x64-11.2.0.4.0.zip
RUN cp /opt/app-root/etc/conf.d/oracle/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip /usr/local/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip

RUN yum install -y unzip
#mkdir -p /etc/local /etc/bin
RUN unzip /usr/local/instantclient-basic-linux.x64-11.2.0.4.0.zip -d /usr/local
RUN unzip /usr/local/instantclient-sdk-linux.x64-11.2.0.4.0.zip -d /usr/local
RUN unzip /usr/local/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip -d /usr/local
RUN ln -s /usr/local/instantclient_11_2 /usr/local/instantclient
RUN ln -s /usr/local/instantclient/libclntsh.so.11.1 /usr/local/instantclient/libclntsh.so
RUN ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus


RUN echo 'instantclient,/usr/local/instantclient' | pecl install -f oci8-2.0.11


#RUN echo 'extension=oci8.so' > /etc/php.d/php.ini
#RUN echo 'extension=redis.so' > /etc/php.d/redis.ini
#RUN echo 'extension=igbinary.so' > /etc/php.d/igbinary.ini

RUN yum clean all -y
USER 1001
CMD $STI_SCRIPTS_PATH/usage
