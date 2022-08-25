# Vagrantfile

This is my Vagrantfile file for building a docker swarm of four nodes

This cluster is made up of;
    
  - 2 x Master Nodes
  - 2 x Worker Nodes

This cluster uses the Debian 11 Vagrant VM produced by [Jeff Geerling](https://app.vagrantup.com/geerlingguy/).

## Network Configuration

The VM is configure with 2 network adapters.

  - Adapter 1 is a `NAT` interface.
  - Adapter 2 is a `Host-Only` adapter. This is configured with a 192.168.60.x address.

## Shared Storage

The /vagrant filesystem is enabled and maps to the working directory.

## Configure Docker Swarm

Once the Vm's have been created by vagrant then you will need to initialise the swarm.  This can be done as follows;

  - On `master1` node.
    - Run the below commands.
      - `$ docker swarm init --advertise-addr 192.168.60.101`
      - `$ docker swarm join-token manager`
      - `$ docker swarm join-token worker`
  - On`master2` node.
    - Run the command the was given when you ran the `docker swarm join-token manager` command on the `master1` node. 
  - On nodes `worker1` and `worker2`.
    - Run the command the was given when you ran the `docker swarm join-token worker` command on the `master1` node.
  - To confirm all nodes have joined the swarm, run the below command on one of the master nodes.  
    - `$ docker node ls`

## CentOS 8 Repositories

Due to CentOS Linux 8 going End Of Life (EOL) as of December 31st 2021, You will need to run the below commands on the VM to allow the yum command to work as expected.

  - `cd /etc/yum.repos.d/`
  - `sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*`
  - `sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*`
  - `yum check-update`

## License

MIT

## Author Information

Created in 2022 by Graham Lillico.
