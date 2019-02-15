# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX = "bionic-canonical-64"
BOX_URL = "https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64-vagrant.box"


Vagrant.configure("2") do |config|
  config.vm.box = BOX
  config.vm.box_url = BOX_URL
  config.disksize.size = '20GB'

  config.vm.network "forwarded_port", guest: 80, host: 8080 # nginx fastcgi
  config.vm.network "forwarded_port", guest: 81, host: 8081 # apache fastcgi
  config.vm.network "forwarded_port", guest: 82, host: 8082 # nginx uwsgi
  config.vm.network "forwarded_port", guest: 83, host: 8083 # nginx mapproxy

  config.vm.provider "virtualbox" do |v|
    #v.gui = true
    v.memory = 8192
    v.cpus = 3
    v.customize ["modifyvm", :id, "--vram", "128"]
  end

  # Install the required software
  config.vm.provision "shell",
    path: "provisioning/setup.sh",
    args: ENV['SHELL_ARGS']

  # Run every time the VM starts
  config.vm.provision "shell",
    path: "provisioning/job.sh",
    args: ENV['SHELL_ARGS'],
    run: "always"

end
