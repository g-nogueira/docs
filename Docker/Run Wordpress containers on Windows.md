## Why is this doc necessary?
Containarizing Wordpress is not as straightforward as one would expect. For example, you could follow the steps on [the oficial wp image](https://hub.docker.com/_/wordpress), but the examples does not tell you that it will probably result in an extremelly slow website. This is because ["mounting a Windows folder into a container can be really slow"](https://forums.docker.com/t/performance-of-wordpress-under-docker-when-using-volumes-on-windows-host/96286/2).

To solve this issue, you can create the container volumes on WSL 2.

## How?
### Pre requisites
- Have installed [Docker for Windows](https://www.docker.com/products/docker-desktop/)
- Enable [Docker's WSL 2 Integration](https://docs.docker.com/desktop/wsl/)

### Steps
You can follow the article [Use WSL2 And Docker To Develop Your WordPress Site On Windows 10](https://www.most-useful.com/wsl2-and-docker-to-develop-your-wordpress-site.html) or this one. This document is based on the aforementioned article.  
1. Open your WSL 2 terminal
2. Create the project's folder.
```bash
mkdir -p ~/Projects/MyProject1 ; cd ~/Projects/MyProject1
```
3. Open VS Code on the directory
```bash
code .
```
In the directory: 

4. Create the docker-compose.yml
```yaml
version: '3.1'
services:
  db:
    image: mariadb:latest
    volumes:
      - ./db:/var/lib/mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: examplepass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress_user
      MYSQL_PASSWORD: examplepass

    wordpress:
    image: wordpress:latest
    depends_on:
      - db
    ports:
      - "8000:80"  # Change the host port as needed
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress_user
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - ./wp:/var/www/html

volumes:
  wordpress:
  db:
```
5. Create the containers
```bash
docker-compose up
```