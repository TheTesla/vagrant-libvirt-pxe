# -*- mode: ruby -*-
# vi: set ft=ruby :

# Every Vagrant development environment requires a box. You can search for
# boxes at https://atlas.hashicorp.com/search.
#BOX_IMAGE = "ubuntu/bionic64"
#BOX_IMAGE = "generic/ubuntu1804"
BOX_IMAGE = "debian/stretch64"
NODE_COUNT = 3
#PROVIDER = "virtualbox"
PROVIDER = "libvirt"


Vagrant.configure("2") do |config|
  config.vm.define "master" do |subconfig|
    subconfig.vm.provider PROVIDER do |v|
      v.cpus = 3
      v.memory = 4096
    end
    subconfig.vm.box = BOX_IMAGE
    subconfig.disksize.size = '42G'
    subconfig.vm.hostname = "master"
    #subconfig.vm.network :public_network, :bridge => "virtestbr3" # for virtualbox
    subconfig.vm.network :public_network, :mode => "bridge", :type => "bridge", :dev => "virtestbr3" # for libvirt
  end
  
  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.provider PROVIDER do |v|
        v.boot 'network'
        v.cpus = 1
        v.memory = 2048
      end
      #subconfig.vm.box = BOX_IMAGE
      subconfig.disksize.size = '23G'
      subconfig.vm.hostname = "node#{i}"
      #subconfig.vm.network :public_network, :bridge => "virtestbr3" # for virtualbox
      subconfig.vm.network :public_network, :mode => "bridge", :type => "bridge", :dev => "virtestbr3" # for libvirt

    end
  end

  # Install avahi on all machines  
  config.vm.provision "shell", inline: <<-SHELL
    apt install -y python
  SHELL
end

