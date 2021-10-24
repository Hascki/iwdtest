# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    # Base VM OS configuration.
    config.vm.box = "geerlingguy/ubuntu2004"
    config.ssh.insert_key = false
    config.vm.boot_timeout = 1000
    config.vm.synced_folder '.', '/vagrant', disabled: true
    # General VirtualBox VM configuration.
    config.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--memory", 512]
        v.customize ["modifyvm", :id, "--cpus", 1]
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end
    # Wordpress.
    config.vm.define "web" do |wordpress|
        wordpress.vm.hostname = "wordpress.dev"
        wordpress.vm.network :private_network, ip: "192.168.28.2"
        wordpress.vm.provision "shell",
            inline: "sudo yum update -y"
        
        wordpress.vm.provider :virtualbox do |v|
            v.customize ["modifyvm", :id, "--memory", 256]
        end
    end
    # db.
    config.vm.define "db" do |db|
        db.vm.hostname = "db.dev"
        db.vm.network :private_network, ip: "192.168.28.3"
        
        # memcached.vm.provision "ansible" do |ansible|
        #     ansible.playbook = "configure.yml"
        #     ansible.inventory_path = "inventories/vagrant/inventory"
        #     ansible.limit = "all"
        #     ansible.extra_vars = {
        #         ansible_ssh_user: 'vagrant',
        #         ansible_ssh_private_key_file: "~/.vagrant.d/insecure_private_key"
        #     }
        # end
    end
end