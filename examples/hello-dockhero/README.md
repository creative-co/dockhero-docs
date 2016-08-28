Example Dockhero Stack
=======================

This example starts a NodeJS webserver to serve files from html folder

A very similar stackis generated with `heroku dh:install v2` command. The difference is 
that `dh:install` references an image from Docker Hub, while the current example builds the image from sources

```
version: '2'
services:
  web:
    image: dockhero/dockhero-docs:hello
    ports:
      - "80:8080"
networks:
  default:
    driver: bridge
```    
