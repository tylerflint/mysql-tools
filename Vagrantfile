#!/usr/bin/env ruby

require 'berkshelf/vagrant'
require 'vagrant-vbguest' unless defined? VagrantVbguest::Config

Vagrant::Config.run do |config|

  config.berkshelf.config_path = File.expand_path('../cookbooks/knife.rb', __FILE__)

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.customize ["modifyvm", :id, "--cpus", 1, "--memory", 512]
  config.vm.share_folder("v-root", "/vagrant", ".")
  config.vbguest.auto_update = true

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = [ 'cookbooks' ]
    chef.add_recipe 'ruby'
    chef.add_recipe 'mysql::server'
    chef.add_recipe 'mysql::client'
    chef.json = {
      "ruby" => {
        "gems" => ['args_parser', 'mysql2']
      },
      "mysql" => {
        "server_root_password" => "root",
        "client" => {
          "packages" => ["mysql-client", "libmysqlclient-dev"]
        }
      }
    }
  end

end