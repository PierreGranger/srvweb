FROM debian:stretch

RUN apt-get update
RUN apt-get install -y software-properties-common

RUN apt-get update -y
RUN apt-get install -y ca-certificates apt-transport-https

RUN apt-get install -y wget gnupg unzip git vim

#users
RUN useradd -ms /bin/bash apidae-event
RUN useradd -ms /bin/bash git
RUN useradd -ms /bin/bash md

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

RUN apt-get install -y memcached
RUN apt-get install -y php7.4-memcached

RUN wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

RUN a2enmod rewrite
RUN a2enmod userdir

RUN service apache2 restart

EXPOSE 80 443

WORKDIR /home/apidae-event/httpdocs

RUN rm -f /run/apache2/apache2.pid
CMD apachectl  -DFOREGROUND -e info