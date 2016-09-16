RethinkDB with Dockhero
========================

This doc explains the quick and easy way to run RethinkDB as a microservice attached to your Heroku app.

First, sign up for free alpha at [dockhero.io](https://dockhero.io/)

Then install the add-on and CLI plugin:

```bash
heroku addons:create dockhero
heroku plugins:install dockhero
```

Now generate `dockhero-compose.yml` using our repository of examples:

```
heroku dh:generate rethinkdb
```

The stack depends on two environment variables which we should set within Heroku:

```
heroku config:set RETHINKDB_PASSWORD=<db connection password>
heroku config:set RETHINKDB_ADMIN_PASSWORD=<admin UI password>
```

Now we're ready to launch the stack using docker-compose:
```
heroku dh:compose up -d
```

You should be able to open admin UI in the browser and login with `admin:RETHINKDB_ADMIN_PASSWORD` credentials

```
heroku dh:open https
```

Connecting from Heroku
----------------------

The address of RethinkDB host is exposed to `DOCKHERO_HOST` environment variable.
Here is a Ruby example using NoBrainer ORM:

```
NoBrainer.configure do |config|
  config.rethinkdb_urls = ["rethinkdb://admin:#{ENV.fetch('RETHINKDB_PASSWORD')}@#{ENV.fetch('DOCKHERO_HOST')}:28015/<dbname>"]
end
```
