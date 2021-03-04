# Welcome to my Docker Project Repository for my SysAdmin II class!

Above you'll find the source code to my Docker Webserver that has several containers such as MariaDB, PHP, and NGINX.

This Webserver is basic right now, but is extremely versatile, as the containers that are linked to it enable any SysAdmin or DEVOPS professional to take this and run. As I learn more about Docker and how to configure this server, I'm sure even I could make it into something extremely useful. It is currently hosted on my docker01 box on my vSphere LAN and can be accessed by any system on the nicholas.local domain.

Configuring this webserver wasn't extremely difficult. To start, the user must install docker and the compose extension.
Then, you need to make a new directory (```mkdir {dir_name}```) and create a docker-compose.yml file in that directory(```touch docker-compose.yml```).
After, run the command ```docker run --name {any_name}.com nazarpc/webserver:data-v1```. This adds the nazarpc/webserver container to your docker system and enables the usage of the following containers.
Once that is done, you open the file in any kind of editor you like (I personally use nano) and add the following configurations:

```
version: '2'
services:
  data:
    image: nazarpc/webserver:data-v1
    volumes_from:
      - container:example.com
  
  logrotate:
    image: nazarpc/webserver:logrotate-v1
    restart: always
    volumes_from:
      - data
  
  mariadb:
    image: nazarpc/webserver:mariadb-v1
    restart: always
    volumes_from:
      - data
  
  nginx:
    image: nazarpc/webserver:nginx-v1
    links:
      - php
#    ports:
#      - {ip where to bind}:{port on previous ip where to bind}:80
    restart: always
    volumes_from:
      - data
  
  php:
    image: nazarpc/webserver:php-fpm-v1
    links:
      - mariadb:mysql
    restart: always
    volumes_from:
      - data
```

The commented out sections under nginx are to set up the IP and port that the server will run through. I used the IP address of my docker01 box and port 80.

Save that file and then use firewalld to permanently open port 80/tcp.
After that, you should be good to rock and roll!

I consulted Nazar PC's gude on awesomeopensource.com. It was an excellent guide and will answer any questions that I missed!
https://awesomeopensource.com/project/nazar-pc/docker-webserver
