#Simple Rails App

This app serves as a playground for various rails features. Currently it is
capable of testing basic rails server provisioning using chef/vagrant, as
well as deployment using Capistrano.

##Capistrano Setup

This repo uses the [capistrano-ruby cookbook](https://github.com/adamkrone/chef-capistrano-ruby)
to provision the servers.

To get a local cluster setup for Capistrano deployment, first spin up the
vagrant machines:

    $ vagrant up

By default, this will setup staging, production, and db servers. You can
target only the ones you care about testing:

    $ vagrant up staging db

This will only bring the staging and db servers online.

The servers are accessible at the following IP addresses:

* staging - 172.16.30.10
* production - 172.16.30.11
* db - 172.16.30.20

Once the machines have come online and the Chef setup has completed, you
will need to copy your ssh public key to the machines you want to deploy
to:

    $ ssh-copy-id deploy@172.16.30.10
    $ ssh-copy-id deploy@172.16.30.11
    $ ssh-copy-id deploy@172.16.30.20

The default deployment user is 'deploy' (which can be configured using
chef attributes). You will be prompted for a password, which is also 'deploy'
by default.

Check if you have any keys setup for agent forwarding:

    $ ssh-add -l

Add your key if you don't have any listed:

    $ ssh-add

You should now be ready to deploy!

    $ bundle exec cap staging deploy

    or

    $ bundle exec cap production deploy
