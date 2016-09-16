Example Dockhero Stack
=======================

This example starts a NodeJS webserver to serve files from html folder

```
version: '2'
services:
  web:
    image: dockhero/dockhero-docs:hello
    ports:
      - "80:8080"
```

