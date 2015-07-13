# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.network "forwarded_port", guest: 80, host: 8081
  
  config.vm.provider "virtualbox" do |vb|
     # Customize the amount of memory on the VM:
     vb.memory = "2048"
  end
  
  config.vm.synced_folder "mainsite", "/var/www/mainsite"
  
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.synced_folder_opts = { type: :nfs }
    config.cache.auto_detect = false
    config.cache.enable :apt
    config.cache.enable :gem
    config.cache.enable :npm
  end

  # SSH setup
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  
  config.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
  end
end
