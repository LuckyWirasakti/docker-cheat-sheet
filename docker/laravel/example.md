### LARAVEL


### Dockerfile

    ```
        FROM php:7.4-fpm

        LABEL maintainer="lucky.wirasakti@icloud.com"

        ARG user
        ARG uid

        RUN apt-get update && apt-get install -y \
            git \
            curl \
            libpng-dev \
            libonig-dev \
            libxml2-dev \
            zip \
            unzip

        RUN apt-get install -y \
                libzip-dev \
                zip \
        && docker-php-ext-install zip

        RUN apt-get clean && rm -rf /var/lib/apt/lists/*

        RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

        RUN pecl install -o -f redis &&  rm -rf /tmp/pear &&  docker-php-ext-enable redis

        COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

        RUN useradd -G www-data,root -u $uid -d /home/$user $user
        RUN mkdir -p /home/$user/.composer && \
            chown -R $user:$user /home/$user

        WORKDIR /var/www

        USER $user

        EXPOSE 9000
        CMD ["php-fpm"]

    ```

### docker-compose.yml

    ```
    version: "3.7"
    services:
    site:
        build:
        args:
            user: example
            uid: 1000
        context: ./
        dockerfile: Dockerfile
        image: example
        container_name: example-site
        restart: unless-stopped
        working_dir: /var/www/
        depends_on:
        - database
        - webserver
        - cache
        volumes:
        - ./:/var/www
        - ./docker-compose/php/local.ini:/usr/local/etc/php/conf.d/local.ini
        networks:
        - example_network

    database:
        image: mysql:5.7.32
        container_name: example-db
        restart: unless-stopped
        environment:
        MYSQL_DATABASE: example
        MYSQL_ROOT_PASSWORD: secret
        volumes:
        - ./docker-compose/mysql:/docker-entrypoint-initdb.d
        networks:
        - example_network

    webserver:
        image: nginx:stable-alpine
        container_name: example-nginx
        restart: unless-stopped
        tty: true
        ports:
        - "3000:80"
        volumes:
        - ./:/var/www
        - ./docker-compose/nginx/conf.d/:/etc/nginx/conf.d/
        networks:
        - example_network

    cache:
        container_name: example-redis
        image: "redis:5.0-alpine"
        restart: unless-stopped
        networks:
        - example_network

    networks:
    example_network:
        driver: bridge

    ```