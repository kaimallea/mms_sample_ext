# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "dev"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo DEBIAN_FRONTEND=noninteractive \
    apt-get update && apt-get install -y \
        git \
        build-essential \
        cmake \
        g++-multilib \
        lib32z1 \
        lib32z1-dev \
        libc6-i386 \
        libc6-dev-i386
  SHELL
end