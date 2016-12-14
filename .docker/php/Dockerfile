FROM centos:latest

MAINTAINER Vicent Girbés <vgirbes@mobincube.com>
WORKDIR /

#Centos update
RUN yum -y update && yum clean all

#Php installation
RUN yum -y install http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm
RUN yum -y update
RUN yum -y install php70u php70u-pdo php70u-mysqlnd php70u-opcache php70u-xml php70u-mcrypt php70u-gd php70u-devel php70u-mysql php70u-intl php70u-mbstring php70u-bcmath php70u-json php70u-iconv php70u-soap
RUN php -v

#Apache installation and config.
RUN yum -y install httpd
RUN mkdir /var/www/bdd
WORKDIR /etc/httpd/
RUN mkdir sites-available
RUN mkdir sites-enabled
COPY .docker/php/bdd.conf /etc/httpd/sites-available
COPY .docker/php/bdd.conf /etc/httpd/sites-enabled
RUN echo IncludeOptional sites-enabled/*.conf >> conf/httpd.conf

#Composer & Laravel installation
RUN yum -y install git-all
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === 'aa96f26c2b67226a324c27919f1eb05f21c248b987e6195cad9690d5c1ff713d53020a02ac8c217dbf90a7eacc9d141d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN composer global require "laravel/installer"

#PHPUnit installation
RUN yum -y install wget
RUN wget https://phar.phpunit.de/phpunit.phar 
RUN chmod +x phpunit.phar
RUN mv phpunit.phar /usr/local/bin/phpunit

WORKDIR /var/www/bdd

#port
EXPOSE 80

#Entry point to keep docker active.
ENTRYPOINT ["/usr/sbin/httpd"]
CMD ["-D", "FOREGROUND"]
