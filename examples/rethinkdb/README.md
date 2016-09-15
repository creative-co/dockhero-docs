RethinkDB with Dockhero
========================

This doc explains the quick and easy way to run RethinkDB as a microservice attached to your Heroku app.

First, sign up for free alpha at [dockhero.io](https://dockhero.io/)

Then install the add-on and CLI plugin:

```bash
heroku addons:create dockhero
heroku plugins:install dockhero
heroku dh:wait
```

Copy **dockhero-compose.yml** into the root of your Heroku application repo on your local machine. 
This file tells Docker which image to run and which ports to expose.

Try it in foreground mode first:

```
heroku dh:compose up -d
```

You should be able to login to the web interface now

```
heroku dh:open 8080
```

**IMPORTANT** Your RethinkDB installation is not secure yet. 
Please see official [Security Guidelines](https://www.rethinkdb.com/docs/security/).
