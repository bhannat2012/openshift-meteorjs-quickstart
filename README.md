#Meteor.js on OpenShift
Deploy meteor.js application bundles on OpenShift

If this is your first time using OpenShift Online or meteor.js, skip down to the "[Basic Setup Notes](https://github.com/openshift-quickstart/openshift-meteorjs-quickstart#basic-setup-notes)" below.

## Configure your OpenShift gear
Spin up a new OpenShift gear with [Node.js](http://nodejs.org), [MongoDB](http://www.mongodb.org/), and [a shim to help meteor.js connect to the correct ports](https://github.com/openshift-quickstart/openshift-meteorjs-quickstart).  This example uses the application name: "**meteor**"

    rhc app create meteor nodejs mongodb-2.2 --from-code=https://github.com/openshift-quickstart/openshift-meteorjs-quickstart.git

The above command will output a local copy of your OpenShift application source in a folder matching your application name.  Be sure to run this command from within a folder where you would like to keep your project source.

## Create a Meteor.js example project
To see a list of all available meteor.js example projects, type `meteor create --list`.

In this guide, we'll use the meteor.js "leaderboard" example:

    meteor create --example leaderboard

See [http://meteor.com/examples/](http://meteor.com/examples/) to learn about what other example applications are available.  Feel free to try them all.

## Bundle and merge your meteor.js code
Bundle up your meteor.js source:

    cd leaderboard # if you chose the leaderboard example
    meteor bundle bundle.tar.gz # to prep for deployment

Add the resulting code bundle to your OpenShift application source folder (`../meteor`):

    tar -xvkf bundle.tar.gz --transform 's|^bundle/||' --directory ../meteor/

The above example assumes that you named your OpenShift application "meteor", as shown in the `rhc app create` step.

Add these new files to your OpenShift application's Git repo:

    cd ../meteor
    git commit -am "Adding a meteor.js application bundle"

## Deploy to OpenShift
Then, push the new code to OpenShift to deploy your meteor.js application bundle:

    git push

That's it! Check out your new Meteor.js application at:

    http://meteor-$yournamespace.rhcloud.com

## Basic Setup Notes
You'll need an OpenShift Online account, and the `rhc` command-line tools in order to follow this guide.  You'll also need to have [Node.js](http://nodejs.org), [MongoDB](http://mongodb.org), and **Meteor** available in your application developement environment. 

### Installing meteor.js

    curl https://install.meteor.com | sh

### OpenShift Online Setup
In this quickstart guide, we'll be using OpenShift Online to host our application.

Sign up for an account at http://openshift.redhat.com/app/account/new

If you don't already have the `rhc` [(Red Hat Cloud) command-line tools](https://openshift.redhat.com/community/get-started#cli), install them:

    sudo gem install rhc

You'll need to run `rhc setup` to link your OpenShift Online account with your local development environment, and to select an application namespace:

    rhc setup

If you need any additional assistance setting up `rhc`, this doc may come in handy: https://openshift.redhat.com/community/developers/rhc-client-tools-install
