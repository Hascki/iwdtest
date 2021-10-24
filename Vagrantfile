# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    # Base VM OS configuration.
    config.vm.box = "geerlingguy/ubuntu1804"
    config.ssh.insert_key = false
    config.vm.boot_timeout = 1000
    config.vm.synced_folder '.', '/vagrant', disabled: true
    # General VirtualBox VM configuration.
    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 4096]
        v.customize ["modifyvm", :id, "--cpus", 2]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
    # db.
    config.vm.define "db" do |db|
        db.vm.hostname = "db.dev"
        db.vm.network :private_network, ip: "192.168.28.3"
        db.vm.provision "ansible" do |ansible|
            ansible.playbook = "./db.yml"
            ansible.inventory_path = "./hosts"
        end
    end
    # Wordpress.
    config.vm.define "web" do |wordpress|
        wordpress.vm.hostname = "wordpress.dev"
        wordpress.vm.network :private_network, ip: "192.168.28.2"
        wordpress.vm.network "forwarded_port", guest: 80, host: 8888
        wordpress.vm.provision "ansible" do |ansible|
            ansible.playbook = "./wp.yml"
            ansible.inventory_path = "./hosts"
        end
    end
end