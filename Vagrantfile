# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

DOMAIN="vagrant.develop"

Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true # true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "ipa.#{DOMAIN}" do |node|
    node.vm.hostname = "ipa.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
    end
  end
  config.vm.define "client1.#{DOMAIN}" do |node|
    node.vm.hostname = "client1.#{DOMAIN}"
    node.vm.provider :libvirt do |domain|
      domain.memory = 2048
      domain.cpus = 2
    end
    node.vm.provision "ansible" do |ansible|
      ansible.limit = "all"
      ansible.playbook = "provisioning/vagrant-playbook.yml"
      ansible.groups = {
        "idm-server" => ["ipa.#{DOMAIN}"],
        "client" => ["client1.#{DOMAIN}"],
      }
    end
  end
end
