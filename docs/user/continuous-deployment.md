---
title: Continuous Deployment
layout: en
permalink: continuous-deployment/
---

Here is a collection of example [.travis.yml](/docs/user/build-configuration/) configurations for deploying your application after a successful build.

<div id="toc"></div>

## Anynines

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "https://api.any9app.com"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## AppFog

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "https://api.ironfoundry.me"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## cloudControl

More infos: [cloudControl addon](/docs/user/addons/cloud-control/).

    addons:
      cloud_control:
        api_key: "your key"  # can be encrypted with `travis encrypt`
        app:     "myapp"     # optional, defaults to project name

## Cloud Foundry

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "http://api.cloudfoundry.com"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## Cloudn PaaS

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "http://api.cloudnpaas.com"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## dotCloud

More infos: [dotCloud addon](/docs/user/addons/dotcloud/).

    addons:
      dotcloud:
        api_key: "your key"  # can be encrypted with `travis encrypt`
        app:     "myapp"     # optional, defaults to project name

## Engine Yard

More infos: [Engine Yard addon](/docs/user/addons/engine-yard/).

    addons:
      engine_yard:
        api_key: "your key"  # can be encrypted with `travis encrypt`
        app:     "myapp"     # optional, defaults to project name

## Google App Engine

More infos: [App Engine addon](/docs/user/addons/app-engine/).

    addons:
      app_engine:
        email:    "someone@example.com"
        password: "foo bar"  # can be encrypted with `travis encrypt`

## Heroku

More infos: [Heroku addon](/docs/user/addons/heroku/).

    addons:
      heroku:
        api_key: "your key"  # can be encrypted with `travis encrypt`
        app:     "myapp"     # optional, defaults to project name

## MoPaaS

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "http://api.mopaas.com"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## Nodejitsu

More infos: [Nodejitsu documentation](https://www.nodejitsu.com/documentation/features/webhooks/#travis).

    notifications:
      webhooks:
        urls: ["http://webhooks.nodejitsu.com/1/deploy"]
        on_success: always
        on_failure: never

## Uhuru AppCloud

More infos: [Cloud Foundry addon](/docs/user/addons/cloud-foundry/).

    addons:
      cloud_foundry:
        target:   "http://api.uhurucloud.com"
        email:    "someone@example.com"
        password: "foo bar"   # can be encrypted with `travis encrypt`
        app:      "myapp"     # optional, defaults to project name

## Custom Deploy via Capistrano

    after_success:
      - chmod 600 .travis/deploy_key.pem # this key should have push access
      - ssh-add .travis/deploy_key.pem
      - gem install capistrano
      - cap deploy

See ["How can I encrypt files that include sensitive data?"](/docs/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data%3F) if you don't want to commit the private key unencrypted to your repository.

## Custom Deploy via FTP

    env:
      global:
        - "FTP_USER=user"         # can be encrypted via `travis encrypt`
        - "FTP_PASSWORD=password" # can be encrypted via `travis encrypt`
    after_success:
        "wget -r ftp://server.com/ --user $FTP_USER --password $FTP_PASSWORD"

## Custom Deploy via Git

This should also work with services like Heroku.

    after_success:
      - chmod 600 .travis/deploy_key.pem # this key should have push access
      - ssh-add .travis/deploy_key.pem
      - git remote add deploy DEPLOY_REPO_URI_GOES_HERE
      - git push deploy

See ["How can I encrypt files that include sensitive data?"](/docs/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data%3F) if you don't want to commit the private key unencrypted to your repository.
