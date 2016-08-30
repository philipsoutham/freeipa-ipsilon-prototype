# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

DOMAIN="vagrant.develop"

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.box_version = "1607.01"
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true 
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "samba.#{DOMAIN}" do |node|
    node.vm.hostname = "samba.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 1024
      domain.cpus = 1
    end
    node.vm.provider :virtualbox do |domain, override|
      override.vm.network "private_network", ip: "192.168.56.102", :name => "vboxnet0", :adapter => 2
      domain.memory = 1024
      domain.cpus = 1
    end
  end

  config.vm.define "ipa.#{DOMAIN}" do |node|
    node.vm.hostname = "ipa.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
    end
    node.vm.provider :virtualbox do |domain, override0|
      override0.vm.network "private_network", ip: "192.168.56.104", :name => "vboxnet0", :adapter => 2
      domain.memory = 2048
      domain.cpus = 2
    end
  end
  config.vm.define "win10" do |node|
    node.vm.hostname = "win10"
    node.vm.communicator = "winrm"
    node.vm.provider :virtualbox do |domain, override1|
      # override.vm.box = "inclusivedesign/windows10-eval"
      # override.vm.box_version = "0.3.0"
      override1.vm.box = "jhakonen/windows-10-n-pro-en-x86_64"
      override1.vm.box_version = "1.0.0"
      override1.vm.network "private_network", ip: "192.168.56.103", :name => "vboxnet0", :adapter => 2
      domain.memory = 2048
      domain.cpus = 2
      # domain.gui = true
    end
    node.vm.provider :libvirt do |domain, override2|
      override2.vm.box = "moozer/win10"
      override2.vm.box_version = "0.1.3"
      override2.winrm.username = "vagrant"
      override2.winrm.password = "vagrant"
      domain.memory = 2048
      domain.cpus = 2
    end
  end
  config.vm.define "client1.#{DOMAIN}" do |node|
    node.vm.hostname = "client1.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 1024
      domain.cpus = 1
    end
    node.vm.provider :virtualbox do |domain, override3|
      override3.vm.network "private_network", ip: "192.168.56.113", :name => "vboxnet0", :adapter => 2
      domain.memory = 1024
      domain.cpus = 1
    end
    node.vm.provision "ansible" do |ansible|
      ansible.limit = "all"
      ansible.playbook = "provisioning/vagrant-playbook.yml"
      ansible.groups = {
        "idm-server" => ["ipa.#{DOMAIN}"],
        "client" => ["client1.#{DOMAIN}"],
        "samba-server" => ["ad.#{DOMAIN}"],
      }
    end
  end

end
