# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
BASE_HOSTNAME = "vm-ubuntu"
BASE_IP = "10.0.1."
NUM_MACHINES = 3

# This defines the version of vagrant
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    
    (1..NUM_MACHINES).each do |id|
        config.vm.define "vm-ubuntu#{id}" do |machine|
            machine.vm.box= "ubuntu/trusty64"
            #machine.vm.box = "elastic/ubuntu-16.04-x86_64"
            machine.vm.hostname= "#{BASE_HOSTNAME}#{id}"
            machine.vm.network "private_network", ip: "#{BASE_IP}#{id}"
            #machine.vm.network "public_network", bridge: "wlan0"
            machine.vm.network "public_network", bridge: [
                "wlp1s0",
                "wlan0",
              ]
            # wlp1s0

            # para elastich
            #machine.vm.network :forwarded_port, guest: 9200, host: 9200
            #machine.vm.network :forwarded_port, guest: 5601, host: 5601
            
            # Acceder como: ssh -p 22  vagrant@10.3.2.163
            # redirecciona el puerto 2222 al 22 en la maquina local y que vagrant se encargue de conflictos: auto_correct: true
            machine.vm.network "forwarded_port", guest: 22, host:2222, id: "ssh", auto_correct: true

            machine.vm.provider :virtualbox do |v|
                v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

                if id == 1
                    v.customize ["modifyvm", :id, "--memory", 3024]
                else
                    v.customize ["modifyvm", :id, "--memory", 1536]
                end 

                # v.customize ["modifyvm", :id, "--memory", 512]
                v.customize ["modifyvm", :id, "--name", "#{BASE_HOSTNAME}#{id}"]
                v.customize ["modifyvm", :id, "--ioapic", "on"]
                v.customize ["modifyvm", :id, "--cpus", 1]
            end
            machine.ssh.insert_key= false
            # machine.vm.provision "shell", path: "script.sh"

            # if id == NUM_MACHINES
            #     machine.vm.provision :shell, inline: "Finished ubuntu/trusty64"
            # end
        end
    end
end



# create with >vagrant init, run with >vagrant up, run each one >vagrant ssh vmubuntu1
