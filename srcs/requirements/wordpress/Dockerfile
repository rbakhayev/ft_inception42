FROM alpine:3.19

# Definisci argomenti
ARG PHP_VERSION=83 \
    DB_NAME \
    DB_USER \
    DB_PASS

# Aggiorna e installa pacchetti necessari
RUN apk update && apk upgrade && apk add --no-cache \
    php${PHP_VERSION} \
    php${PHP_VERSION}-fpm \
    php${PHP_VERSION}-mysqli \
    php${PHP_VERSION}-json \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-dom \
    php${PHP_VERSION}-exif \
    php${PHP_VERSION}-fileinfo \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-openssl \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-redis \
    wget \
    unzip && \
    # Modifica i file di configurazione per PHP-FPM
    sed -i "s|listen = 127.0.0.1:9000|listen = 9000|g" \
      /etc/php${PHP_VERSION}/php-fpm.d/www.conf && \
    sed -i "s|;listen.owner = nobody|listen.owner = nobody|g" \
      /etc/php${PHP_VERSION}/php-fpm.d/www.conf && \
    sed -i "s|;listen.group = nobody|listen.group = nobody|g" \
      /etc/php${PHP_VERSION}/php-fpm.d/www.conf && \
    # Pulisce la cache apk
    rm -rf /var/cache/apk/*

# Imposta la directory di lavoro
WORKDIR /var/www

# Scarica e configura WordPress
RUN wget https://wordpress.org/latest.zip && \
    unzip latest.zip && \
    cp -rf wordpress/* . && \
    rm -rf wordpress latest.zip

# Copia lo script di configurazione di WordPress
COPY ./requirements/wordpress/conf/wp-config-create.sh .

# Esegui lo script di configurazione di WordPress
RUN sh wp-config-create.sh && rm wp-config-create.sh && \
    chmod -R 0777 wp-content/

# Comando per eseguire PHP-FPM
CMD ["php-fpm83", "-F"]

