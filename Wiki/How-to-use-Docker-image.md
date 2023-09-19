# How to use Docker image

## Lightweight usage with embedded database

For very easy startup just use: **!!!You should change path to your local videos!!!**

`docker run -d -p 8080:8080 -v /path/to/local/videos:/data --name=streama gkiko/streama` and browse <http://localhost:8080/>

This way embedded database would persist inside the same directory, so you can restart it anytime.

## Advanced usage with external database

For more advanced usage you could use external mysql database.

### Pure docker way

`docker run -it -p 8080:8080 -v /path/to/local/videos:/data -e ACTIVE_PROFILE=mysql -e MYSQL_HOST=_MYSQL_HOST_ -e MYSQL_PORT=_MYSQL_PORT_ -e MYSQL_DB=_MYSQL_DB_NAME_ -e MYSQL_USER=_MYSQL_USERNAME_ -e MYSQL_PASSWORD=_MYSQL_PASSWORD_ --name=streama gkiko/streama`

Configuration is self-explained. You should change `/path/to/local/videos` to your local videos path.

### Docker-compose way

You could do the configuration in a more readable way with docker-compose:

```docker-compose
version: '3'

services:
  streama:
    image: gkiko/streama:latest
    ports:
      - 8080:8080
    volumes:
      - /path/to/local/videos:/data # !!! CHANGE THIS TO YOU LOCAL VIDEO STORE PATH !!!
    environment:
      ACTIVE_PROFILE: mysql
      # ----------
      # Change config below
      # ----------
      MYSQL_HOST: _MYSQL_HOST_
      MYSQL_PORT: _MYSQL_PORT_
      MYSQL_DB: _MYSQL_DB_NAME_
      MYSQL_USER: _MYSQL_USERNAME_
      MYSQL_PASSWORD: _MYSQL_PASSWORD_
```

You can also use docker-based mysql like this:

```docker-compose
version: '3'

services:
  mysql:
    image: mysql:5.7
    volumes:
      - /path/to/db/data:/var/lib/mysql # CHANGE THIS TO LOCAL DATABASE PATH
    expose:
      - 3306
    environment:
      MYSQL_ROOT_PASSWORD: streama_root_password
      MYSQL_USER: streama
      MYSQL_DATABASE: streama
      MYSQL_PASSWORD: streama_password

  streama:
    image: gkiko/streama:latest
    ports:
      - 8080:8080
    volumes:
      - /path/to/local/videos:/data # !!! CHANGE THIS TO YOU LOCAL VIDEO STORE PATH !!!
    depends_on:
      - mysql
    environment:
      ACTIVE_PROFILE: mysql
      MYSQL_HOST: mysql
      MYSQL_PORT: 3306
      MYSQL_DB: streama
      MYSQL_USER: streama
      MYSQL_PASSWORD: streama_password
```

Just run `docker-compose up -d` and browse <http://localhost:8080>


_Written by @l33tness_