RethinkDB with Dockhero
========================

First, copy **dockhero-compose.yml** into the root of your Heroku application repo on your local machine. 
This file tells Docker which image to run and which ports to expose.

Try it in foreground mode first:

```
heroku dh:compose up
```

You should see RethinkDB logs on your console. 

Stop it with Ctrl-C, wait for 15 seconds, then run in background mode:

```
heroku dh:compose start
```

You should be able to login to the web interface now

```
heroku dh:open 8080
```

**IMPORTANT** Your RethinkDB installation is not secure yet. 
Please see official [Security Guidelines](https://www.rethinkdb.com/docs/security/).
To add password-protection to the Web UI, comment "port 8080" line and uncomment **proxy** section in dockhero-compose.yml
