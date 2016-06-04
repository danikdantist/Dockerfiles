FROM debian:stable
ENV HOME=/root

RUN apt-get update

#php-fpm
RUN apt-get install -y php5-fpm php5-pgsql php5-curl php5 php5-redis php5-gd php5-mcrypt php5-imap php5-dev php-apc php5-idn

#nginx
RUN apt-get install -y nginx-extras

#kafka
ENV LIB_RDKAFKA_VERSION=0.9.1

RUN apt-get install -y build-essential python libssl-dev libsasl2-dev libicu-dev zip unzip

RUN cd /tmp \
    && curl -SL https://github.com/edenhill/librdkafka/archive/confluent-debian-${LIB_RDKAFKA_VERSION}.tar.gz -o librdkafka-${LIB_RDKAFKA_VERSION}.tgz \
    && tar xvzpf librdkafka-${LIB_RDKAFKA_VERSION}.tgz \
    && cd librdkafka-confluent-debian-${LIB_RDKAFKA_VERSION} \
    && ./configure \
    && make -j "$(getconf _NPROCESSORS_ONLN)" \
    && make install

RUN cd /tmp \
    && curl -LOk https://github.com/arnaud-lb/php-rdkafka/archive/master.zip \
    && unzip master.zip \
    && cd php-rdkafka-master \
    && phpize \
    && ./configure \
    && make all -j "$(getconf _NPROCESSORS_ONLN)" \
    && make test \
    && make install


RUN echo 'extension=rdkafka.so' > /etc/php5/mods-available/rdkafka.ini && \
    cd /etc/php5/cli/conf.d && \
    ln -s ../../mods-available/rdkafka.ini 20-rdkafka.ini && \
    cd /etc/php5/fpm/conf.d && \
    ln -s ../../mods-available/rdkafka.ini 20-rdkafka.ini



ENTRYPOINT /bin/bash