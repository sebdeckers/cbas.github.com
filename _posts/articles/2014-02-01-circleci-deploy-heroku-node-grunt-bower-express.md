---
title: Deploying Grunt and Bower from CircleCI to Heroku with Node.js
abstract: How to set up an automated frontend app stack using these popular tools.
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---

## The App

The frontend app is purely static files: auto-compressed images, minified and concatenated Javascript, pre-processed CSS, and so on.

The frontend talks to a backend exclusively through an API that allows CORS. This strict separation means the backend can be developed and deployed independently. We don't need to consider the backend, nor database, when setting up the frontend deployment.

## Running Tests with Grunt

Every good app deserves automated tests.

Services like TravisCI and CircleCI automatically run `npm test`. Adhering to convention, I set up my `package.json` to map the command onto `grunt test`. This affords me the full power and flexibility of Grunt.

**package.json snippet:**
{% highlight json %}
"scripts": {
  // Map `npm test` to `grunt test` for extra awesome
  "test": "grunt test"
},
"dependencies": {
  // Make the `grunt` command available from the command line,
  // but only within our project directory.
  "grunt-cli": "*",
  "grunt": "0.4.x"
}
{% endhighlight %}

**Gruntfile.js snippet:**
{% highlight js %}
grunt.registerTask('test', [
  'clean:tests',
  'jshint',
  'nodeunit',
  'stage',
  'connect:staging',
  'ghost'
]);
{% endhighlight %}

## Continuous Integration with CircleCI

I've used TravisCI and CircleCI on commercial, production apps. There are many others, like Codeship, which I will not cover in this tutorial. Even good old Jenkins can be used, if you can take the pain of configuring and maintaining it.

I use CircleCI to test commits on any branch. CircleCI automatically runs `npm install` and `npm test` for any project with a `package.json` file. Before running tests, I tell it to run `bower install` which retrieves the frontend dependencies.

Upon a successful test, CircleCI can deploy the app to Heroku. Following Git Flow conventions, the `master` branch deploys to my production app, and the `develop` branch to my staging app.

**circle.yml**
{% highlight yaml %}
test:
  pre:
    - bower install
deployment:
  production:
    branch: master
    heroku:
      appname: my-app-production
  staging:
    branch: develop
    heroku:
      appname: my-app-staging
{% endhighlight %}

**Note:** Change `my-app-production` and `my-app-staging` to your respective Heroku app names.

## Deploying to Heroku

Heroku has two ways to configure how your app will deploy:

1. **Procfile**

   This file is placed in the root of the repository. It contains the command to launch the app. The app must listen on port 80 within 60 seconds, otherwise the command is terminated. Avoid running any build steps or tests here.

1. **Buildpacks**

   These are repositories containing shell scripts that run all the necessary build steps. Whenever CircleCI deploys to your Heroku instance, these scripts will run.

### Custom Buildpack

The official Node.js buildpack only runs `npm install`. So I configure my Heroku apps with the community-maintained buildpack for Grunt. Crucially, it executes `npm install` and `grunt heroku` before Heroku runs `Procfile`.

{% highlight bash %}
heroku config:set BUILDPACK_URL=https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt.git --app my-app-staging
heroku config:set BUILDPACK_URL=https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt.git --app my-app-production
{% endhighlight %}

### Grunt & Bower

The buildpack runs `grunt heroku`, which is used to run the necessary tasks that will fetch dependencies and generate the static files. I use the `grunt-bower-task` package to install Bower dependencies.

**package.json snippet:**
{% highlight json %}
dependencies: {
  "bower": "*",
  "grunt-bower-task": "0.3.4"
}
{% endhighlight %}

**Gruntfile snippet:**
{% highlight js %}
grunt.initConfig({
  bower: {
    install: {
    }
  }
});

grunt.registerTask('heroku', [
  'bower', // Runs `bower install` to fetch frontend dependencies
  'default' // Builds the static app files: HTML, JS, CSS, images, etc.
]);
{% endhighlight %}

### Procfile

Finally launch a web server to host the static files.

**Gruntfile snippet:**
{% highlight js %}
grunt.initConfig({
  connect: {
    production: {
      options: {
        port: process.env.PORT || 80
      }
    }
  }
});
{% endhighlight %}

**Procfile**
{% highlight yaml %}
web: grunt connect:production:keepalive
{% endhighlight %}

## Results

### Never Ship a Broken Build

With this automation in place, CircleCI tells me whenever a build fails to pass the tests.

![CircleCI build report](http://i.imgur.com/mbGpJg5.png)

### Easily Debug Build Problems

The build logs are always recorded in CircleCI for review and debugging.

![CircleCI deploy log](http://i.imgur.com/9uD35Lm.png)

### Clean Runtime Logs on Heroku

Prevent the build output from spamming the Heroku logs.

![Heroku log](http://i.imgur.com/YRDddTA.png)
