# -*- mode: ruby -*-
# vi: set ft=ruby :

$INSTALL_DOCKER_SCRIPT = <<SCRIPT
echo "Installing dependencies ..."
sudo apt-get update
echo Installing Docker...
curl -sSL https://get.docker.com/ | sh
sudo usermod -aG docker vagrant
SCRIPT

VAGRANTFILE_API_VERSION = "2"
VAGRANT_BOX_NAME = "geerlingguy/debian11"
VAGRANT_MEMORY = "512"
VAGRANT_CPUS = 1

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = VAGRANT_BOX_NAME

  config.vm.provider :virtualbox do |v|
    v.memory = VAGRANT_MEMORY
    v.cpus = VAGRANT_CPUS
    v.linked_clone = true
  end

  # Define the VMs with static private IP addresses.
  boxes = [
    { :name => "master1", :ip => "192.168.60.101" },
    { :name => "master2", :ip => "192.168.60.102" },
    { :name => "worker1", :ip => "192.168.60.103" },
    { :name => "worker2", :ip => "192.168.60.104" },
  ]

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]
      config.vm.provision "shell",inline: $INSTALL_DOCKER_SCRIPT, privileged: true
    end
  end
end
