FROM debian:stretch

RUN apt-get update -y
RUN apt-get install -y software-properties-common
RUN apt-get install -y ca-certificates apt-transport-https
RUN apt-get install -y wget gnupg unzip git vim

RUN alias ll='ls -alF'

#users
RUN useradd -ms /bin/bash apidae-event
RUN useradd -ms /bin/bash git
RUN useradd -ms /bin/bash md
RUN useradd -ms /bin/bash demo
RUN useradd -ms /bin/bash roadmap
RUN useradd -ms /bin/bash admin

# Ajout des utilisateurs (2) aux groupes (1)
RUN usermod -a -G git apidae-event
RUN usermod -a -G git md

RUN wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add -
RUN echo "deb https://packages.sury.org/php/ stretch main" | tee /etc/apt/sources.list.d/php.list
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg

RUN apt-get update -y
RUN apt-get install -y apache2

RUN apt-get install -y php7.4 php7.4-cli php7.4-common php7.4-curl php7.4-mbstring php7.4-xml php7.4-mysqli
RUN apt-get install -y php7.4-gd php7.4-zip php7.4-intl php7.4-sqlite3
RUN apt-get install -y php7.4-pgsql

RUN apt-get install -y memcached
RUN apt-get install -y php7.4-memcached

RUN wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

RUN mkdir /etc/apache2/ssl
WORKDIR /etc/apache2/ssl

RUN apt-get install -y openssl
RUN openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -subj "/C=FR/ST=AU/L=Montoldre/O=Apidaetourisme/OU=Apidae Tourisme/CN=git.apidae-tourisme.local" -keyout /etc/apache2/ssl/git.apidae-tourisme.local.key -out /etc/apache2/ssl/git.apidae-tourisme.local.crt
RUN openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -subj "/C=FR/ST=AU/L=Montoldre/O=Apidaetourisme/OU=Apidae Tourisme/CN=admin.apidae-tourisme.local" -keyout /etc/apache2/ssl/admin.apidae-tourisme.local.key -out /etc/apache2/ssl/admin.apidae-tourisme.local.crt

RUN openssl dhparam -out /etc/ssl/certs/dhparam.pem 204

RUN a2enmod rewrite
RUN a2enmod userdir
RUN a2enmod ssl

RUN service apache2 restart

EXPOSE 80
EXPOSE 443

RUN rm -f /run/apache2/apache2.pid
CMD apachectl  -DFOREGROUND -e info