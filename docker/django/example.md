### LARAVEL


### Dockerfile

    FROM python:3.8.7
    LABEL maintainer="lucky.adhikrisna@erasysconsulting.com"

    # env var
    ENV PYTHONUNBUFFERED=1

    # copy host to container
    COPY . /code/

    # working direcory
    WORKDIR /code/

    # install module third party
    RUN apt install -y libmariadb-dev-compat libmariadb-dev
    RUN pip install -r requirements.txt

    # expose port
    EXPOSE 8000

    # start bash command
    ENTRYPOINT ["sh","start.sh"]


### docker-compose.yml

    version: "3.7"
    
    services:
        core-database:
            image: mysql:5.7.32
            container_name: database_chat
            restart: always
            environment: 
            - MYSQL_ROOT_PASSWORD=secret
            - MYSQL_DATABASE=core
            networks:
            - core-networks

        core-site:
            build: .
            restart: on-failure
            container_name: site_chat
            volumes:
            - .:/code/
            ports:
            - "8000:8000"
            depends_on:
            - core-database
            networks:
            - core-networks

    networks:
    core-networks:
        driver: bridge
