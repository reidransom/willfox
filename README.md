# willfox

General usage here is to push a local git repo to a remote repo at webfaction, then use the git post-update hook to kick off the `willfox` backup and update scripts.

`willfox` will be installed on the webfaction server (not on the client).    The only thing you need installed on the client is `git`.

## Installation

SSH into your Webfaction account and run the command:

    curl -fsSL http://url.to/willfox > ~/bin/willfox && chmod 755 ~/bin/willfox

## Setup

First, create your web app via the Webfaction web-based admin interface.  For this example, we'll call it `nodeapp_prod`.

#### On the server

    $ cd ~/webapps/nodeapp_prod
    $ willfox init

This will setup your git repo on the server along with a post-commit hook that runs `willfox update`.

#### On the client

Add the remote by cloning:

    $ git clone user@server:~/webapps/nodeapp_prod/repo.git

... or by init:

    $ mkdir nodeapp
    $ cd nodeapp
    $ git init
    $ git remote add origin user@server:~/webapps/nodeapp_prod/repo.git

Then later you write some code and commit like:

    $ touch README
    $ git add README
    $ git commit -m "Example."
    $ git push origin master

## Usage

`willfox` is an executable python script that should be installed on the webfaction server in a place like `~/bin/willfox`.

    $ willfox init|update|revert [<app_name>]

Then you can run commands (especially from the git post-update hook) like this:

    $ willfox update [<webapp>]

By default webapp will be determined by the cwd.

---

`willfox update` will stop the server process, backup the codebase, update the codebase, run `npm install --production`, and start the server process.

`willfox revert` will stop the server process, delete the codebase, copy the most recent backup codebase, and start the server process.
