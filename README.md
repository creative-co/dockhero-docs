[DockHero](http://addons.heroku.com/dockhero) hosts your docker stack in the cloud.

Please request private alpha access at [dockhero.io](http://dockhero.io/)

Run any image from Docker Hub in our infrastructure, and attach it to your Heroku app as if it was a Heroku add-on.
Each docker image runs on a separate Amazon Web Services instance with a dedicated IP.
Your docker image logs will appear among your Heroku app’s logs.

> callout
> The add-on is currently in private testing and available to the alpha testers only.
> Please email us at [dockhero@castle.co](mailto:dockhero@castle.co) to get access.

## Provisioning the add-on

DockHero can be attached to a Heroku application via the CLI:

> callout
> A list of all plans available can be found [here](http://addons.heroku.com/dockhero).

```term
$ heroku addons:create dockhero
-----> Adding dockhero to sharp-mountain-4005... done, v18 (free)
```

Once DockHero has been added a `DOCKHERO_HOST` setting will be available in the app configuration and will contain the hostname of the newly provisioned Docker Swarm cluster (consisting of a single machine). This can be confirmed using the `heroku config:get` command.

```term
$ heroku config:get DOCKHERO_HOST
orange-apple-1984.dockhero.io
```

## Installing CLI plugin

In order to access the newly provisioned cluster, install Dockhero CLI plugin:

```term
  heroku plugins:install dockhero
```

Now you can run docker / docker-compose against that cluster using the shortcuts below:

```term
  heroku dh:docker
  heroku dh:compose
```

Find other usage examples of the plugin in the [docs](https://github.com/cloudcastle/dockhero-cli)


## Preparing a stackfile

Use the following command to generate a sample stackfile:

```
$ heroku dh:install
Writing example stack into dockhero-compose.yml...
```

This will create  **dockhero-compose.yml** in the root of your repo. 
It's recommended that you also put this file under version control.
See [docker-compose.yml reference](https://docs.docker.com/v1.8/compose/yml/) for the file syntax.
There are also some [stackfile examples](https://github.com/cloudcastle/dockhero-docs/tree/master/examples) for your reference.

In our example below, we'll be building a pluggable resource which returns message of the day via HTTP.
Here's a [Docker image](https://hub.docker.com/r/dockhero/motd-http/) which does that.

> callout
> You should already have Docker client set up locally.
> Please follow the [installation instructions for your platform](https://docs.docker.com/installation/)

First, replace `dockhero-compose.yml` content with the following:

```yml
web:
  image: dockhero/motd-http
  ports:
    - "80:8000"
```

Then try out the stack in the foreground mode with the following command:

```
$ heroku dh:compose up
Starting motdhttp_web_1...
Attaching to motdhttp_web_1  
```

This will launch our containerized app on a remote cluster and attach it to the local terminal, so that  
you could see any logs or error messages from your app in the console. 
Make sure the app starts successfully, then stop it by pressing `Ctrl-C`, and restart in production (detached) mode:

```term
$ heroku dh:compose scale web=1
Starting motdhttp_web_1...
```

Now you should be able to see message of the day by opening the app in your browser

```term
$ heroku dh:open
Opening http://orange-apple-1984.dockhero.io/...
```

## Using with Ruby / Rails

In a Ruby / Rails application on Heroku, you can fetch the host name of your Docker service from DOCKHERO_HOST environment variable. With the stack above spinned up, you could read the MOTD message using the following Ruby code:

```ruby
#!/usr/bin/env ruby
response = open("http://#{DOCKHERO_HOST}/") { |f| f.read }
puts "DOCKHERO says:\n  #{response}"
```

## Using SSL Endpoint

Each Dockhero cluster comes with CloudFlare endpoint configured in *full ssl* mode. It has a valid SSL certificate installed, so that you don't need to purchase one yourself.
SSL is terminated at the CloudFlare edge server, then it is encrypted again, and sent back to Dockhero cluster all encrypted. This is illustrated by this image from CloudFlare docs:

![Full SSL](https://support.cloudflare.com/hc/en-us/article_attachments/206167937/cfssl_full.png)

Your stack still needs to talk SSL, but you can use a self-signed certificate, like in [this example](#). No worries, the users will see a valid CloudFlare's certificate - find more about CloudFlare SSL in [this article](https://support.cloudflare.com/hc/en-us/articles/200170416-What-do-the-SSL-options-mean-)


## Monitoring & Logging

DockHero activity can be identified within the Heroku log-stream by `dockhero` prefix:

```term
$ heroku logs -t | grep 'dockhero'
```

## Migrating between plans

Currently only the Test plan is supported

## Removing the add-on

DockHero can be removed via the CLI.

> warning
> This will destroy all associated data and cannot be undone!

```term
$ heroku addons:destroy dockhero
-----> Removing dockhero from sharp-mountain-4005... done, v20 (free)
```


## Support

All DockHero support and runtime issues should be submitted via one of the [Heroku Support channels](support-channels). For other questions or suggestions, please email us at [dockhero@castle.co](mailto: dockhero@castle.co) or use in-app support chat in the bottom-right corner of add-on dashboard.

If you prefer GitHub way, please feel free to file an issue [here](https://github.com/cloudcastle/dockhero/issues)

You can improve the current docs or post your own stackfile examples by forking [our repo](https://github.com/cloudcastle/dockhero/) and sending a pull request.
