# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # servers to work with
  servers = ["nagios", "webserver", "snmphost"]

  # set come vm options
  config.vm.box = "debian/stretch64"
  config.vm.synced_folder "shared", "/vagrant", disabled: true

  # start this on the localhost, not a remote kvm host
  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2

    libvirt.uri = "qemu:///system"
    libvirt.management_network_name = "vagrant-libvirt"
    libvirt.management_network_address = "192.168.121.0/24"

  end

  # define provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "test.yml"
    ansible.groups = {
          "servers" => ["nagios", "webserver"],
          "infrastructure"  => ["snmphost"]
    }
  end

  # spin up servers
  servers.each do |server_name|
    config.vm.define server_name do |machine|
          machine.vm.hostname = server_name
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
