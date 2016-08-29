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
  config.hostmanager.manage_host = true # true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "samba.#{DOMAIN}" do |node|
    node.vm.hostname = "samba.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 1024
      domain.cpus = 1
    end
    node.vm.provider :virtualbox do |domain|
      node.vm.network "private_network", virtualbox__intnet: true
      domain.memory = 1024
      domain.cpus = 1
    end
    # node.vm.provision "ansible" do |ansible|
    #   ansible.limit = "all"
    #   ansible.playbook = "provisioning/vagrant-playbook.yml"
    #   ansible.groups = {
    #     "samba-server" => ["samba.#{DOMAIN}"],
    #   }
    # end
  end

  config.vm.define "ipa.#{DOMAIN}" do |node|
    node.vm.hostname = "ipa.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
    end
    node.vm.provider :virtualbox do |domain, override|
      override.vm.network "private_network", virtualbox__intnet: true
      domain.memory = 2048
      domain.cpus = 2
    end
  end
  config.vm.define "win10.#{DOMAIN}" do |node|
    node.vm.hostname = "win10"
    node.vm.communicator = "winrm"
    node.vm.provider :virtualbox do |domain, override|
      override.vm.box = "inclusivedesign/windows10-eval"
      override.vm.box_version = "0.3.0"
      # override.vm.box = "jhakonen/windows-10-n-pro-en-x86_64"
      # override.vm.box_version = "1.0.0"
      override.vm.network "private_network", virtualbox__intnet: true
      domain.memory = 2048
      domain.cpus = 2
    end
    node.vm.provider :libvirt do |domain, override|
      override.vm.box = "moozer/win10"
      override.vm.box_version = "0.1.3"
      override.winrm.username = "vagrant"
      override.winrm.password = "vagrant"
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
    node.vm.provider :virtualbox do |domain, override|
      override.vm.network "private_network", virtualbox__intnet: true
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
