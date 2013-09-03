---
title: Cloud Foundry Deployment
layout: en
permalink: cloudfoundry/
---

Travis CI can automatically deploy your [Cloud Foundry](http://www.cloudfoundry.com/) application after a successful build.

For a minimal configuration, all you need to do is add the following to your `.travis.yml`:

    deploy:
      provider: cloudfoundry
      username: "YOUR USERNAME"
      password: "YOUR PASSWORD"
      target: "YOUR TRAGET URI"
      api_key: "YOUR API KEY"
      organization: "YOUR ORGANIZATION"
      space: "SPACE"

It is recommended to encrypt the password. Assuming you have the Travis CI command line client installed, you can set up everything like this:

    $ travis setup cloudfoundry

Keep in mind that the above command has to run in your project directory, so it can modify the `.travis.yml` for you.


### Branch to deploy from

If you have branch specific options, as [shown below](#Deploy-different-Space), Travis CI will automatically figure out which branches to deploy from. Otherwise, it will only deploy from your **master** branch.

You can also explicitly specify the branch to deploy from with the **on** option:

    deploy:
      provider: cloudfoundry
      ...
      on: production

Alternatively, you can also configure it to deploy from all branches:

    deploy:
      provider: cloudfoundry
      ...
      on:
        all_branches: true

Builds triggered from Pull Requests will never trigger a deploy.

### Deploy different Spaces

It is also possible to deploy different branches to different spaces:

    deploy:
      ...
      space:
        master: staging
        production: production

### Conditional Deploys

It is possible to make deployments conditional using the **on** option:

    deploy:
      ...
      on:
        branch: staging
        rvm: 2.0.0

The above configuration will trigger a deploy if the staging branch is passing on Ruby 2.0.0.

You can also add custom conditions:

    deploy:
      ...
      on:
        condition: "$CC = gcc"

Available conditions are:

* **all_branches** - when set to true, trigger deploy from any branch if passing
* **branch** - branch or list of branches to deploy from if passing
* **condition** - custom condition or list of custom conditions
* **jdk** - JDK version to deploy from if passing
* **node** - NodeJS version to deploy from if passing
* **perl** - Perl version to deploy from if passing
* **php** - PHP version to deploy from if passing
* **python** - Python version to deploy from if passing
* **ruby** - Ruby version to deploy from if passing
* **repo** - only trigger a build for the given repository, to play nice with forks
