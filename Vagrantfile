# -*- mode: ruby -*-
# # vi: set ft=ruby :
Vagrant.configure("2") do |config|

  config.vm.box = "coreos"
  config.vm.box_url = "http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_vagrant.box"

  # Uncomment below to enable NFS for sharing the host machine into the coreos-vagrant VM.
  config.vm.network "private_network", ip: "172.12.8.150"
  config.vm.synced_folder ".", "/home/core/share", id: "core", :nfs => true,  :mount_options   => ['nolock,vers=3,udp']
    #config.vm.synced_folder "~/Documents/development/jump-backend", "/home/core/backend", :nfs => true, :mount_options   => ['nolock,vers=3,udp']
    #config.vm.synced_folder "~/Documents/development/jump-devops", "/home/core/devops", :nfs => true, :mount_options   => ['nolock,vers=3,udp']

  # configure port forwarding as needed throughout
  config.vm.network "forwarded_port", guest: 4243, host: 4243
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 5001, host: 5001
  config.vm.network "forwarded_port", guest: 5002, host: 5002


  # Fix docker not being able to resolve private registry in VirtualBox
  config.vm.provider :virtualbox do |vb, override|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provider :vmware_fusion do |vb, override|
    override.vm.box_url = "http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_vagrant_vmware_fusion.box"
  end

  # plugin conflict
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  config.vm.provision :shell, :inline => "sudo cp /home/core/share/docker-local.service /media/state/units"
  config.vm.provision :shell, :inline => "sudo systemctl restart local-enable.service"

end
