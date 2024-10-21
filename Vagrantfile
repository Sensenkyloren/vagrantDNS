# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.box_check_update = false

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt upgrade -y
  SHELL

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
  end

  # Máquina 1: DNS Maestro - tierra.sistema.test
  config.vm.define "dns_master" do |dns_master|
    dns_master.vm.hostname = "tierra.sistema.test"
    dns_master.vm.network "private_network", ip: "192.168.57.103", virtualbox__intnet: true
    
    dns_master.vm.provision "shell", inline: <<-SHELL
      apt-get install -y bind9 bind9utils bind9-doc
      cp /vagrant/Master/named.conf /etc/bind/named.conf
      cp /vagrant/Master/sistema.test.db /etc/bind/sistema.test.db
      cp /vagrant/Master/57.168.192.in-addr.arpa /etc/bind/57.168.192.in-addr.arpa
      systemctl restart bind9
    SHELL
  end

  # Máquina 2: DNS Esclavo - venus.sistema.test
  config.vm.define "dns_slave" do |dns_slave|
    dns_slave.vm.hostname = "venus.sistema.test"
    dns_slave.vm.network "private_network", ip: "192.168.57.102", virtualbox__intnet: true
    
    dns_slave.vm.provision "shell", inline: <<-SHELL
      apt-get install -y bind9 bind9utils bind9-doc
      cp /vagrant/Slave/named.conf /etc/bind/named.conf
      systemctl restart bind9
    SHELL
  end
end
