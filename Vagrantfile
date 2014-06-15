# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'chef/ubuntu-12.04'

  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  config.vm.define 'staging' do |web|
    web.vm.network 'private_network', ip: '172.16.30.10'

    web.vm.provision 'chef_solo' do |chef|
      chef.data_bags_path = 'data_bags'
      chef.add_recipe 'chef-solo-search'
      chef.add_recipe 'capistrano-ruby::web-app-role'
      chef.add_recipe 'rvm::vagrant'

      chef.json = {
        :rvm => {
          :vagrant => {
            :system_chef_solo => '/opt/chef/bin/chef-solo'
          }
        },
        :capistrano_ruby => {
          :environment => 'staging'
        }
      }
    end
  end

  config.vm.define 'production' do |web|
    web.vm.network 'private_network', ip: '172.16.30.11'

    web.vm.provision 'chef_solo' do |chef|
      chef.data_bags_path = 'data_bags'
      chef.add_recipe 'chef-solo-search'
      chef.add_recipe 'capistrano-ruby::web-app-role'
      chef.add_recipe 'rvm::vagrant'

      chef.json = {
        :rvm => {
          :vagrant => {
            :system_chef_solo => '/opt/chef/bin/chef-solo'
          }
        }
      }
    end
  end

  config.vm.define 'db' do |db|
    db.vm.network 'private_network', ip: '172.16.30.20'

    db.vm.provision 'chef_solo' do |chef|
      chef.data_bags_path = 'data_bags'
      chef.add_recipe 'chef-solo-search'
      chef.add_recipe 'capistrano-ruby::postgresql-role'
      chef.add_recipe 'rvm::vagrant'

      chef.json = {
        :rvm => {
          :vagrant => {
            :system_chef_solo => '/opt/chef/bin/chef-solo'
          }
        },
        :capistrano_ruby => {
          :db => {
            :user => 'simple_rails_app',
            :user_password => 'simplerailsapp',
            :name => 'simple_rails_app',
            :environments => ['staging', 'production']
          }
        }
      }
    end
  end
end
