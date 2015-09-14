# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.network :private_network, ip: '192.168.50.50'
  config.vm.network "forwarded_port", guest: 80, host: 8082
  
  config.vm.provider "virtualbox" do |v|
	  host = RbConfig::CONFIG['host_os']

	  # VM 1/4 system memory & access to all cpu cores on the host
	  if host =~ /darwin/
	    # sysctl returns Bytes and we need to convert to MB
	    mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
	  elsif host =~ /linux/
	    # meminfo shows KB and we need to convert to MB
	    mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
	  end

	  v.customize ["modifyvm", :id, "--memory", mem]
	end
  
  config.vm.synced_folder "webroot", "/var/www/webroot", nfs: true
  
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.synced_folder_opts = { type: :nfs }
    config.cache.auto_detect = false
    config.cache.enable :apt
    config.cache.enable :gem
    config.cache.enable :npm
  end
  
  config.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yml"
  end
end
