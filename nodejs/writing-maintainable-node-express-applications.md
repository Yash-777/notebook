Building Maintainable Node/Express Applications
===============================================

If you aren't already familiar with building web application using Node and Express, it's way too big to talk about here. But if you have a little experience - even just followed a tutorial or two on the topic - then the principles and practices described below will help you write your application in a way that makes it easy to maintain. And whether that maintenance is going to be done by you a month or two from now or the next guy, everyone wins when that task becomes easier.

### Refer To The Documentation (RTTD) ###

These are something else.

https://www.npmjs.com/package/blocked
http://livereload.com/
https://www.airpair.com/node.js/posts/top-10-mistakes-node-developers-make
https://strongloop.com/strongblog/compare-node-js-logging-winston-bunyan/
https://github.com/baryshev/look
https://www.npmjs.com/package/morgan-debug
http://kb.imakewebsites.ca/2014/01/04/new-node-wishlist/

# Start With Express-Generator #

use it as a starting point, but don't limit yourself to what it provides

# Develop With `nodemon`, Deploy With `forever` #

nodemon is great in development. forever does something very similar, but in production

# Parameterize Your Environment #

the express application should know what environment it's in (development, test, production, etc) and alter it's functionality accordingly. similarly, global variables that differ between environments should be stored in environment specific configuration files

# Use Configuration Files #

speaking of configuration files, modules that require configuration should have their own configuration scripts (mongoose, passport, seraph, etc.) that get called by the start script

# Be Idiomatic #

the start script (server.js) should call the required configuration scripts and start the server in around 100 lines, should be easy to read, and should rarely be modified

# Find Stuff Easier #

remembering all those paths sucks. there are better ways. use them
https://gist.github.com/branneman/8048520

read in your apps folder https://www.npmjs.com/package/app-module-path

# Convention Over Configuration #

the majority of feature/functionality changes should not require a developer to modify the start or configuration scripts. follow conventions to load them synchronously
