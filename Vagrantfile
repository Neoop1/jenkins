# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.define "ControlServer" do |controlserver|
    controlserver.vm.box = "ubuntu/focal64"
    controlserver.vm.network "private_network", ip: "192.168.100.100"
    controlserver.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlserver.vm.hostname =  "controlserver"
    controlserver.vm.provider "virtualbox" do |vb|
       vb.memory = "2024"
     end
     config.vm.provision "file", source: 'sshkey/ssh_key', destination: "/home/vagrant/.ssh/"
     config.vm.provision "file", source: 'sshkey/ssh_key.pub', destination: "/home/vagrant/.ssh/"
     controlserver.vm.provision "shell", inline: <<-SHELL
       sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
       service ssh restart
       #sudo apt install python3-pip -y
       #sudo add-apt-repository --yes --update ppa:ansible/ansible
       #sudo apt update && apt --assume-yes install ansible
       chmod 600 /home/vagrant/.ssh/ssh_key
       cat /home/vagrant/.ssh/ssh_key.pub >> /home/vagrant/.ssh/authorized_keys
       #ansible-galaxy install -r /home/vagrant/ansible/requirements.yml
       SHELL
   end
 
   config.vm.define "redhatclient" do |redhatclient|
    redhatclient.vm.box = "bento/centos-7.5"
    redhatclient.vm.network "private_network", ip: "192.168.100.102"
    redhatclient.vm.synced_folder "./ansible/","/home/vagrant/ansible"
    redhatclient.vm.hostname =  "redhatclient"
    redhatclient.vm.provider "virtualbox" do |vb|
       vb.memory = "1024"
     end
     config.vm.provision "file", source: 'sshkey/ssh_key', destination: "/home/vagrant/.ssh/"
     config.vm.provision "file", source: 'sshkey/ssh_key.pub', destination: "/home/vagrant/.ssh/"
     redhatclient.vm.provision "shell" , inline: <<-SHELL
       sed -i -e '$aPermitRootLogin yes' /etc/ssh/sshd_config
       sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
       service sshd restart
       chmod 600 /home/vagrant/.ssh/ssh_key
       cat /home/vagrant/.ssh/ssh_key.pub >> /home/vagrant/.ssh/authorized_keys
       SHELL
   end
 
   config.vm.define "debiantomcat" do |debiantomcat|
    debiantomcat.vm.box = "debian/buster64"
    debiantomcat.vm.network "private_network", ip: "192.168.100.110"
    #debiantomcat.vm.network "forwarded_port", guest: 80, host: 8080
    debiantomcat.vm.hostname =  "debiantomcat"
    debiantomcat.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    config.vm.provision "file", source: 'sshkey/ssh_key', destination: "/home/vagrant/.ssh/"
    config.vm.provision "file", source: 'sshkey/ssh_key.pub', destination: "/home/vagrant/.ssh/"
    debiantomcat.vm.provision "shell" , inline: <<-SHELL
      sed -i -e '$aPermitRootLogin yes' /etc/ssh/sshd_config
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      chmod 600 /home/vagrant/.ssh/sshkey/ssh_key.pub
      cat /home/vagrant/.ssh/sshkey/ssh_key.pub >> /home/vagrant/.ssh/authorized_keys
      SHELL
  end
end
