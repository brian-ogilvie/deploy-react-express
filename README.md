# How to Deploy a React/Express app to Heroku

These are the steps to follow so that you don't lose a day (or week) with frustrating bugs.

## Server.js

3 changes related to Node 'path':

```
// top of file
const path = require('path')

// after logger and bodyParser
app.use('/', express.static('./build/'))

// after all routes, before app.listen()
if (process.env.NODE_ENV == "production") {
  app.get("/*", function(request, response) {
    response.sendFile(path.join(__dirname, "build", "index.html"));
  });
}
```

Also, be sure that PORT is set as follows:

`const PORT = process.env.PORT || 5000`

## package.json

```
"engines": {
	"node": "11.9.0",
	"npm": "6.9.0"
},
```

Be sure to use `node -v` and `npm -v` to get the correct version numbers.

## Sequelize: config/config.json

```
"production": {
	"use_env_variable": "DATABASE_URL",
	"dialect": "postgres"
}
```

## Create deploy branch

`git checkout -b deploy`

## heroku cli

Create your app on heroku:

`heroku create <<app-name>>`

Set up env variables:

`heroku config:set API_KEY=<<whatever>>`

Provision the database:

`heroku addons:create heroku-postgresql:hoby-dev`

Push to heroku branch:

`git push heroku deploy:master`

## Migrate Database

`heroku run sequelize db:migrate`


