## Selamat Datang di DOCKER CHEATSHEET

Halaman ini diterbitkan sebagai catatan kombinasi perintah umum yang digunakan pada lingkungan kerja kontainer docker, siapapun bebas untuk berkontribusi dalam proyek ini jika berkenan.

### BUILD

Membuat image dari dockerfile disertai dengan nama image dan tag.

    docker build -t image-name:tag .

### RUN

Membuat container disertai dengan nama image, penerusan local port, bind volume container to host, dan network name

    docker run -it -p port-expose:port-local --name container-name -v "$(pwd)/:/workdir/" --network network-name .

Menjalankan container 

    docker start container-name
    
Memulai ulang container

    docker restart container-name
    
Menghentikan container

    docker exec -it container-name bash

### Thanks to
- [Docker](http://docker.com/)
- Jekyll Themes


### Support or Contact
Anda dapat menghubungi saya melalui email [```lucky.wirasakti@icloud.com```](mailto:lucky.wirasakti@icloud.com)
