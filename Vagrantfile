# -*- mode: ruby -*-
# vi: set ft=ruby :
## The preceding lines are used to tell your text editor that this is written with ruby

##################
#Global Variables#
##################

#Network Settings
IP_ADDRESS = '172.22.22.22'
PROJECT_NAME = 'project-name'

#Passwords
DATABASE_PASSWORD = 'password'
LINUX_PASSWORD = ''

#Versions
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = 'ubuntu/trusty64'

  # Use hostonly network with a static IP Address and enable
  # hostmanager so we can have a custom domain for the server
  # by modifying the host machines hosts file
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.vm.define PROJECT_NAME do |node|
    node.vm.hostname = "#{PROJECT_NAME}.local"
    node.vm.network :private_network, ip: IP_ADDRESS
    node.hostmanager.aliases = [ "www.#{node.vm.hostname}"]
  end
  config.vm.provision :hostmanager

  #Enable berkshelf support for Chef Cookbook Management
  config.berkshelf.enabled = true

  # Use the omnibus installer for the latest Chef installation
  config.omnibus.chef_version = :latest


  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe 'apt'
    chef.add_recipe 'mysql::server'

    chef.json = {
      mysql: {
        server_root_password: DATABASE_PASSWORD,
        server_debian_password: LINUX_PASSWORD,
        allow_remote_root: true
      }
    }
  end


end
