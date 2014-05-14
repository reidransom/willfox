# willfox

General usage here is to push a local git repo to a remote repo at webfaction, then use the git post-update hook to kick off the `willfox` backup and update scripts.

`willfox` will be installed on the webfaction server (not on the client).    The only thing you need installed on the client is `git`.

_Currently only node apps are supported, though more should be easy enough to add._

## Installation

SSH into your Webfaction account and run the command:

    curl https://raw.githubusercontent.com/reidransom/willfox/master/willfox > ~/bin/willfox && chmod 755 ~/bin/willfox

## Setup

First, create your web app via the Webfaction web-based admin interface.  For this example, we'll call it `nodeapp_prod`.

Then __on the server...__

    $ cd ~/webapps/nodeapp_prod
    $ willfox init

This will setup your git repo on the server along with a post-commit hook that runs `willfox update`.

Then __on the client...__

Add the remote by cloning:

    $ git clone user@server:~/webapps/nodeapp_prod/repo.git

... or by init:

    $ mkdir nodeapp
    $ cd nodeapp
    $ git init
    $ git remote add origin user@server:~/webapps/nodeapp_prod/repo.git

## Usage

After the setup, you write some code and commit like:

    $ touch README
    $ git add README
    $ git commit -m "Example."
    $ git push origin master

From here on, every time you `git push`, your web app will be updated and restarted if necessary.

Currently, `willfox update` will stop the server process, backup the codebase, update the codebase, run `npm install --production`, and start the server process.

`willfox revert` will stop the server process, delete the codebase, copy the most recent backup codebase, and start the server process.
