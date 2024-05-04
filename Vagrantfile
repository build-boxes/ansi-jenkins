# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  #### config.vm.box = "raufhammad/debian12"

  # Enable public access Guest VM Port forwarding
  #### config.vm.network "forwarded_port", guest: 8080, host: 8080
  # Share non-vagrant folder on host.
  #### config.vm.synced_folder "../shared_folder", "/mnt/shared_folder"
  # Disable vagrant default share.
  #### config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "debian" do |debian|
    debian.vm.box = "raufhammad/debian12"
    debian.vm.network "forwarded_port", guest: 8080, host: 8080
    debian.vm.synced_folder "../shared_folder", "/mnt/shared_folder"
    debian.vm.synced_folder ".", "/vagrant", disabled: true
  end

  # config.vm.define "rockylin8" do |rockylin8|
  #   rockylin8.vm.box = "raufhammad/rockylinux8"
  #   rockylin8.vm.network "forwarded_port", guest: 8080, host: 8081
  #   rockylin8.vm.synced_folder "../shared_folder", "/mnt/shared_folder"
  #   rockylin8.vm.synced_folder ".", "/vagrant", disabled: true
  # end

  # config.vm.define "centos9" do |centos9|
  #   centos9.vm.box = "raufhammad/centos9"
  #   centos9.vm.network "forwarded_port", guest: 8080, host: 8082
  #   centos9.vm.synced_folder "../shared_folder", "/mnt/shared_folder"
  #   centos9.vm.synced_folder ".", "/vagrant", disabled: true
  # end

  # Copy Host user SSH Pub Key to Guest
  config.vm.provision "file", source: "/home/#{ENV['USER']}/.ssh/id_rsa.pub", destination: "~/.ssh/me.pub"
  ##config.vm.provision "shell", path: "vagrant_script.sh"
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "main.yml"
    ansible.raw_arguments = [
      "--vault-password-file=./vars/.vault_pass"
    ]
  end  
end
