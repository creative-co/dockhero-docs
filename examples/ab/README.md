Apache Benchmark for your Heroku app
================

Once [Dockhero addon](https://elements.heroku.com/addons/dockhero) and [CLI plugin](https://github.com/cloudcastle/dockhero-cli) are installed, you can
run Apache Benchmark remotely like this:

```
$ heroku dh:docker run jordi/ab ab -v 2 https://docker.io/
```

This command will be launched in a Docker container in AWS,
in the same availability zone where your Heroku app is hosted.
