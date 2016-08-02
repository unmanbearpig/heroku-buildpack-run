Heroku Buildpack: Run
=====================

<img src="http://img4.hostingpics.net/pics/708500bprun.png" width="600">

Run custom commands during the build process.


Description
-----------

This buildpack allows to execute arbitrary commands on the build dyno during the build process.

Just create the file `buildpack-run.sh` in the root directory of your application and write in this file the commands that you want to execute. This file is then *sourced* by the `compile` script of this buildpack. That is, the commands in `buildpack-run.sh` are executed on the build dyno as they would be part of the `compile` script.

Available build-specific variables are `STACK`, `BUILD_DIR`, `CACHE_DIR`, and `ENV_DIR`, `BUILDPACK_DIR`.
All config variables are available.

The initial working directory is the root directory of your application.

For aborting the build at any point, you can use the `exit` command with a non-zero exit code, e.g. `exit 1`.

This buildpack is useful for finding out information about the build process.


Usage
-----

Simply do:

~~~bash
# Create file 'buildpack-run.sh' containing bash commands
echo 'echo "hello world"' > buildpack-run.sh

heroku buildpacks:set https://github.com/TheKoon/heroku-buildpack-run.git
~~~

The buildpack is now set and will be used on the next deploy.

For more information on how to use custom buildpacks, see <https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack>.


Usage together with other buildpacks
------------------------------------

You can use multiple buildpacks at the same time as described on [https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app](
https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app):

~~~bash
heroku buildpacks:add --index 1 heroku/php
heroku buildpacks:add --index 2 https://github.com/TheKoon/heroku-buildpack-run.git
~~~

![Multiple Buildpacks](http://img4.hostingpics.net/pics/482392Capturedecran20160801a211657.png)

You can always check which buildpacks you have currently added to your app with:

~~~bash
heroku buildpacks
~~~


Custom script name
------------------

The default name of the source script is `buildpack-run.sh`, but you can set whatever you want:

~~~bash
heroku config:set BUILDPACK_RUN="buildpack-run-staging.sh"
~~~
