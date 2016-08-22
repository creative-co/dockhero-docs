Apache Benchmark for your Heroku app
================

First, install [Dockhero addon](https://elements.heroku.com/addons/dockhero) and [CLI plugin](https://github.com/cloudcastle/dockhero-cli)

```
$ heroku plugins:install dockhero
$ heroku addons:create dockhero  # installation will take some time
```

Docker installation takes some time. You can track installation progress via API:

```
$ heroku dh:wait
```

Once installation finished, run Apache Benchmark remotely like this (replace https://docker.io/ with your Heroku app url):

```
$ heroku dh:docker run jordi/ab ab -v 2 https://docker.io/
```

This command will be launched in a Docker container in AWS,
in the same availability zone where your Heroku app is hosted.
