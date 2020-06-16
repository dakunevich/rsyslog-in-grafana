# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    BOX_IMAGE = "centos/7"
    BOX_IMAGE_VERSION = "2004.01"
    NODE_COUNT = 2

    # General VirtualBox VM configuration.
    config.vm.provider :virtualbox do |v|
        v.memory = 1536
        v.cpus = 2
        v.linked_clone = true
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end    

    config.vm.define "master" do |subconfig|
        subconfig.vm.box = BOX_IMAGE
        subconfig.vm.box_version = BOX_IMAGE_VERSION
        subconfig.vm.hostname = "master"
        subconfig.vm.network :private_network, ip: "192.168.2.10"
        
        subconfig.vm.provider :virtualbox do |v|
            v.name = "master"
        end
    end

    (1..NODE_COUNT).each do |i|
        config.vm.define "node#{i}" do |subconfig|
            subconfig.vm.box = BOX_IMAGE
            subconfig.vm.box_version = BOX_IMAGE_VERSION
            subconfig.vm.hostname = "node#{i}"
            subconfig.vm.network :private_network, ip: "192.168.2.#{i+10}"
            
            subconfig.vm.provider :virtualbox do |v|
                v.name = "node#{i}"
            end
            
            # Only execute once the Ansible provisioner,
            # when all the machines are up and ready.

            # if machine_id == NODE_COUNT
            #     subconfig.vm.provision :ansible do |ansible|
            #     # Disable default limit to connect to all the machines
            #     ansible.limit = "all"
            #     ansible.playbook = "provisioning/playbook.yml"
            #     end
            # end
        end       
    end

end
