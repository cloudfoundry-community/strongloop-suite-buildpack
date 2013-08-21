StrongLoop Node BuildPack
=========================

This repository is a StrongLoop Node buildpack to allow you to run
StrongLoop Node as a runtime "service" on a few different PaaS
environments. Currently supported PaaS environments are:

    SalesForce's Heroku
    Pivotal's Cloud Foundry

Okay, now onto how to use this buildpack.

Deploying on Heroku:
--------------------
On Heroku, first optionally copy and edit the config files as shown above,
then create an app using this buildpack and finally push to your Heroku app.
That's the tl;dr version - detailed steps below.


Create an account on http://heroku.com/

Install the Heroku toolbelt

     See: https://toolbelt.heroku.com/ for instructions

Login into the Heroku:

     heroku login

Clone the sample application or create your application:

     git clone git://github.com/lulinqing/paas-quickstart.git strongloop
     # OR  git clone ${path_to_your_awesome_app}.git strongloop
     # OR  cp -r ~/myapp/*  strongloop

Change directory to your application and optionally copy over the
StrongLoop sample configuration

    cd strongloop
    git add .
    git commit -m 'Added StrongLoop config files'

And when you are satisfied that all's ok, just push your app to Heroku:

    #  Create an app with this buildpack and push to it.
    heroku apps:create -b git://github.com/lulinqing/paas-buildpack.git
    git push heroku master

This will download and configure StrongLoop Node on Heroku, install the
dependencies as specified in the sample application's package.json file
(or npm-shrinkwrap.json if one exists).

That's it, you can now checkout your StrongLoop Node application at the
app url/domain returned from the heroku apps:create command.
    

Deploying on Cloud Foundry:
--------------------------

Create an account on http://run.pivotal.io/

Install the cf command line tools, and check their official CLI guide here:
http://docs.cloudfoundry.com/docs/using/managing-apps/cf/

     sudo gem install cf
     
Login into the Cloud Foundry PaaS:

     cf login

Clone or create your application:

     git clone git://github.com/lulinqing/paas-quickstart.git strongloop
     # OR  git clone ${path_to_your_awesome_app}.git strongloop
     # OR  cp -r ~/myapp/*  strongloop

And when you are satisfied that all's ok, push your app to Cloud Foundry.

    cf push strongloop --buildpack=git://github.com/lulinqing/paas-buildpack.git

Note:  The first time you run cf push, you will need to specify all the
       parameters - this will create a new application at Cloud Foundry.
       domain `example: strongloop.cfapps.io`.
       And save the configuration `example: y`.

Subsequent pushes just need the `cf push` command - you only need to
specify all those options the first time you create the app (push).

This will now download and configure StrongLoop Node on Cloud Foundry,
install the dependencies as specified in the sample application's
package.json file (or npm-shrinkwrap.json if one exists).

That's it, you can now checkout your StrongLoop Node application at the
app url/domain you set for your Cloud Foundry app.


    Copy StrongLoop Node configuration files [OPTIONAL]
    ---------------------------------------------------
    This step is strictly optional - you don't need to copy over StrongLoop
    Node configuration files. However if you wish to control which StrongLoop
    version you want to install + what deployment mode to run in you will need
    to provide StrongLoop specific configuration files, so in such as case
    using the sample configuration is a good start.

        #  Copy the config files to your app in a strongloop/ directory.
        cd myapp
        cp -r $local_path_to_this_buildpack/samples/strongloop .


    Selecting a StrongLoop Node version to use/install
    --------------------------------------------------
    If you add a strongloop directory (with the configuration marker
    files) to your application, then that configuration is automatically used
    by the StrongLoop installer specific to the type of PaaS you are
    deploying on. 

        Example: To install StrongLoop Node version 1.0.0-0.2.beta
        echo -e "1.0.0-0.2.beta\n" >> strongloop/VERSION

    The platform specific installers in this buildpack will use that VERSION
    file to download and extract the specific StrongLoop Node version if it
    is available and automatically setup the paths to use the node/npm
    binaries from the specific install directory.


    Selecting the application's Deployment Mode
    -------------------------------------------
    To select the deployment mode, just edit or add the environment to your
    application in a file named strongloop/NODE_ENV

        Example: To run in production mode
        echo -e "production\n" >> strongloop/NODE_ENV

    The platform specific installers in this buildpack will use that NODE_ENV
    file and set the environment for npm to install and run your application.

    Okay, now onto how you can get a StrongLoop supported Node.js version
    running on the different PaaS environments.
