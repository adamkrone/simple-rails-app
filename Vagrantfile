# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chef/ubuntu-12.04"

  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  config.vm.define "staging" do |web|
    web.vm.network "private_network", ip: "172.16.30.10"
  end

  config.vm.define "production" do |web|
    web.vm.network "private_network", ip: "172.16.30.11"
  end

  config.vm.define "db" do |db|
    db.vm.network "private_network", ip: "172.16.30.20"

    db.vm.provision "chef_solo" do |chef|
      chef.add_recipe "postgresql::server"

      chef.json = {
        :postgresql => {
          :config => {
            :listen_addresses => "*"
          },
          :pg_hba => [
            {
              :addr => "0.0.0.0/0",
              :db => "all",
              :method => "md5",
              :type => "host",
              :user => "all"
            },
            {
              :addr => "::1/0",
              :db => "all",
              :method => "md5",
              :type => "host",
              :user => "all"
            }
          ],
          :password => {
            :postgres => "postgres"
          }
        }
      }
    end
  end
end
