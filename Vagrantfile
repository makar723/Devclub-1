# -*- mode: ruby -*-
# vi: set ft=ruby :
$hostsfile_update = <<-'SCRIPT'
echo -e '192.168.56.110 control.example.com control\n192.168.56.111 node1.example.com node1\n192.168.56.112 node2.example.com node2' >> /etc/hosts
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config && systemctl restart sshd
useradd ansible -m -s /bin/bash
echo "ansible:password" | sudo chpasswd
echo "%ansible ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible
SCRIPT
#$mach_quant = 2


Vagrant.configure("2") do |config|
  config.vm.box_check_update = false
  config.vm.define "control", primary: true do |control|
    control.vm.box = "ubuntu/focal64"
	
	control.vm.hostname = "control.example.com"
    #control.vm.network "forwarded_port", guest: 80, host: 8080
    control.vm.network "private_network", ip: "192.168.56.110"
    control.vm.provision "shell", inline: $hostsfile_update
    control.vm.provider "virtualbox" do |v|
    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
	  vb.customize ["modifyvm", :id, "--audio", "none"]
	  vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
	  vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxsvga"]
	  
    end
 	v.memory = 4096
	v.cpus = 2
	control.vm.provision "shell", inline: <<-SHELL
      #sudo apt update -y
	  sudo apt install software-properties-common -y
	  sudo add-apt-repository --yes --update ppa:ansible/ansible
	  sudo apt install ansible -y
	  mkdir /home/ansible/ansible
	  cp /vagrant/inventory /home/ansible/ansible/inventory
	  cp /vagrant/ansible.cfg /home/ansible/ansible/ansible.cfg
	  cp /vagrant/task1.yml /home/ansible/ansible/task1.yml
	  cp /vagrant/task2.yml /home/ansible/ansible/task2.yml
	  mkdir /home/ansible/ansible/vars
	  cp /vagrant/default.yml /home/ansible/ansible/vars/default.yml
	  mkdir /home/ansible/ansible/files
	  cp /vagrant/site.conf.j2 /home/ansible/ansible/files/site.conf.j2
	SHELL
    end
  end
  
  config.vm.define "node1" do |node1|
    node1.vm.box = "ubuntu/focal64"
    node1.vm.hostname = "node1.example.com"
    node1.vm.network "private_network", ip: "192.168.56.111"
	node1.vm.network "forwarded_port", guest: 80, host: 8080
    node1.vm.provision "shell", inline: $hostsfile_update
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--audio", "none"]
	  vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
	  vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxsvga"]
    end
	
  end
  
  config.vm.define "node2" do |node2|
    node2.vm.box = "ubuntu/focal64"
    node2.vm.hostname = "node2.example.com"
    node2.vm.network "private_network", ip: "192.168.56.112"
    node2.vm.provision "shell", inline: $hostsfile_update
	#node2.vm.provision "shell", inline:  <<-SHELL
	#	sudo apt update -y && sudo apt upgrade -y
	#SHELL
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--audio", "none"]
	  vb.customize ["modifyvm", :id, "--accelerate3d", "off"]
	  vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxsvga"]
    end
	
  end
  
end
