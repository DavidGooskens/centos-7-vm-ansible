# -*- mode: ruby -*-
# vi: set ft=ruby :

# Use config.yml for basic VM configuration.
require 'yaml'
if !File.exist?('./config.yml')
  raise 'Configuration file not found! Please copy example.config.yml to config.yml and try again.'
end
vconfig = YAML::load_file("./config.yml")

Vagrant.configure(2) do |config|
  config.vm.hostname = vconfig['vagrant_hostname']
  config.vm.box = "centos/7"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: vconfig['vagrant_ip']

  # Share an additional folder to the guest VM.
  config.vm.synced_folder vconfig['site_local'], "/var/www/site", :nfs => true

  # Provider-specific configuration.
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = vconfig['vm_ram']
  end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/amp/site.yml"
  end
end
