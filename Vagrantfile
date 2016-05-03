# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "debian/contrib-jessie64"
  config.vm.box_check_update = false

  # 384 MB RAM for all virtualbox VMs
  config.vm.provider "virtualbox" do |vb|
      vb.memory = "384"
  end
  
  
  config.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh", disabled: true

  config.vm.define :vm1 do |vm1|
    vm1.vm.network "private_network", ip: "192.168.23.10"
    vm1.vm.network :forwarded_port, guest: 22, host: 2210, auto_correct: false
  end

  config.vm.define :vm2 do |vm2|
    vm2.vm.network "private_network", ip: "192.168.23.20"
    vm2.vm.network :forwarded_port, guest: 22, host: 2220, auto_correct: false
  end
  
  config.vm.define :vm3 do |vm3|
    vm3.vm.network "private_network", ip: "192.168.23.30"
    vm3.vm.network :forwarded_port, guest: 22, host: 2230, auto_correct: false
  end

  ## check later ##
  
    
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
