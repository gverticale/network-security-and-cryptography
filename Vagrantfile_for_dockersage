require 'securerandom'

random_string = SecureRandom.hex
TOKEN="polimi2"

# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 8888, host: 8888

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  config.vm.network "forwarded_port", guest: 8888, host: 8888, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.provision "docker" do |d|
    d.build_image "/vagrant/setup",
      args: "-t sagecrypto"
  end
  
  config.trigger.after :up do |trigger|
    trigger.name = "Hello world"
    trigger.info = "I am running after vagrant up!!"
    #trigger.run_remote{inline: "sudo -u vagrant docker run -d -p 8888:8888 --env JUPYTER_TOKEN=#{TOKEN} -v /vagrant:/home/sage/notebooks sagecrypto"}
  end
  
  config.vm.post_up_message = "http://localhost:8888/tree?token=#{TOKEN}"

  config.vm.provision "shell", privileged: "false", run: "always" do |s|    
    s.inline = "docker run -d -p 8888:8888 --env JUPYTER_TOKEN=$1 -v /vagrant:/home/sage/notebooks sagecrypto"
    s.args = "#{TOKEN}"
  end

  #config.vm.provision "docker" do |d|
  #  d.run "sagecrypto",
  #    args: "-d -p 8888:8888 --env JUPYTER_TOKEN=#{TOKEN} --mount type=bind,source=/vagrant,target=/home/sage/notebooks"    
  #end
  
end
