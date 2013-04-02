---
title: CasperJS on Travis CI
abstract: Functional and integration testing with PhantomJS
published: true
comments: true
author_email: seb@ninja.sg
author: Sebastiaan Deckers
layout: article
categories:
- articles
---

This is a follow up up on my post about [frontend testing](/frontend-testing/).

If you spend any time on GitHub, you might have come across [Travis CI](https://travis-ci.org/). Travis CI is a service that runs automated tests. It's free for any public repository, and [Travis Pro](http://travis-ci.com/) is currently in beta for private repos.

It's easy to hook up Travis CI to run a command line test suite. I typically run [JSHint](http://jshint.com/), [nodeunit](https://github.com/caolan/nodeunit), and [CasperJS](http://casperjs.org/) on my frontend apps. Travis CI virtual machines offer PhantomJS out of the box but not CasperJS. But of course we can fix that. Here's the pattern I'm using.

Have a look at the [cbas/demo-testing](https://github.com/cbas/demo-testing) repository for a sample app. Below are snippets with explanations of the important parts.

### .travis.yml

Using a `before_install` hook we download CasperJS and add it to the `PATH` environment variable.

```yaml
language: node_js
node_js:
  - "0.8"
before_install:
  - npm install -g grunt-cli
  - git clone git://github.com/n1k0/casperjs.git ~/casperjs
  - cd ~/casperjs
  - git checkout tags/1.0.2
  - export PATH=$PATH:`pwd`/bin
  - cd -
before_script:
  - phantomjs --version
  - casperjs --version
```

### package.json

Hook up `npm test` to `grunt test`:

```json
"scripts": {
	"test": "grunt test"
}
```

And set up the dependencies. *Latest versions at the time of writing. Use [Gemnasium](https://gemnasium.com/) to update.*

```json
"devDependencies": {
	"grunt": "0.4.1",
	"grunt-contrib-connect": "0.2.0",
	"grunt-contrib-jshint": "0.3.0",
	"grunt-contrib-nodeunit": "0.1.2",
	"grunt-ghost": "1.0.8"
}
```

### Gruntfile.js

Import the dependencies.

```js
grunt.loadNpmTasks('grunt-contrib-connect');
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.loadNpmTasks('grunt-contrib-nodeunit');
grunt.loadNpmTasks('grunt-ghost');
```

Set up `test` to invoke our testing frameworks.

```js
grunt.registerTask('test', ['jshint', 'nodeunit', 'connect', 'ghost']);
```

Configure the targets. These are just examples and you should modify them for your project's directory structure.

```js
jshint: {
	options: {
		jshintrc: '.jshintrc'
	},
	files: [
		'Gruntfile.js',
		'package.json',
		'source/**/*.js',
		'<%= nodeunit.tests %>'
	]
},
nodeunit: {
	tests: ['tests/nodeunit/*_test.js']
},
connect: {
	www: {
		options: {
			base: 'source',
			port: 4545
		}
	}
},
ghost: {
	test: {
		files: [{
			src: ['tests/ghost/*_test.js']
		}]
	},
	options: {
		args: {
			baseUrl: 'http://localhost:' +
				'<%= connect.www.options.port %>/'
		},
		direct: false,
		logLevel: 'error',
		printCommand: false,
		printFilePaths: true
	}
}
```

### And one `git push` later...

The result should be something like this:

[![Travis CI screenshot](http://i.imgur.com/69GcHhe.png)](https://travis-ci.org/cbas/demo-testing/builds/5976986)
