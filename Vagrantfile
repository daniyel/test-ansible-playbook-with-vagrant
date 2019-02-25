# -*- mode: ruby -*-
# vi: set ft=ruby :
VM_BOX  = 'ubuntu/bionic64' # Ubuntu 18.04
NETWORK = 'forwarded_port'
GUEST_PORT = 80
HOST_PORT = 9000
GUEST_HOME = '/home/ubuntu/.ssh/authorized_keys'
DISK = './dataDisk1.vdi'

Vagrant.require_version ">= 2.0.0"

Vagrant.configure(2) do |config|
  config.vm.box = VM_BOX
  config.vm.network NETWORK, guest: GUEST_PORT, host: HOST_PORT
  config.vm.provider "virtualbox" do |vb|
    unless File.exist?(DISK)
      # Create new volume with 2GB size
      vb.customize ['createhd', '--filename', DISK, '--variant', 'Fixed', '--size', 2 * 1024]
    end
    vb.memory = 1024
    # Attach new volume
    # NOTICE: Tested just on OSX, for other OS please check, if this setting works
    vb.customize ['storageattach', :id,  '--storagectl', 'SCSI', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', DISK]
  end

  config.vm.provision 'shell' do |s|
    ssh_pub_key = File.readlines("#{Dir.pwd}/authorized_keys").first.strip
    s.inline = <<-SHELL
      apt install -y htop python-minimal
      echo 'ubuntu:ubuntu' | sudo chpasswd
      echo #{ssh_pub_key} >> #{GUEST_HOME}
    SHELL
  end
end
