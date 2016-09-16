Example Dockhero Stack
=======================

Sources for https://hub.docker.com/r/dockhero/hello-dockhero/

This image contains a NodeJS webserver which serves files from html folder

Usage
-----

```
version: '2'
services:
  web:
    image: dockhero/dockhero-docs:hello
    ports:
      - "80:8080"
```

