# php

一个docker版的PHP镜像

## 拉取镜像
```
docker pull flxxyz/php:fpm
or
docker pull flxxyz/php:cli
```

[使用其他版本](#其他版本)

## 编排例子
```
example-fpm:
    image: flxxyz/php:fpm
    restart: always
    volumes:
      - fpm配置文件:/usr/local/etc/php-fpm.d/www.conf
      - 本地站点目录的路径:/var/www/html

example-cli:
    image: flxxyz/php:cli
    restart: always
    volumes:
      - fpm配置文件:/usr/local/etc/php-fpm.d/www.conf
      - 本地站点目录的路径:/var/www/html
    command: php 执行站点目录下的php脚本
```

## 其他版本
- [7.3-fpm, fpm](https://github.com/edog-docker/php/blob/master/fpm/Dockerfile) ✔
- [7.3-cli, cli](https://github.com/edog-docker/php/blob/master/cli/Dockerfile) ✔