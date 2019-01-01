# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

HOSTS = [
  { :name => "test-gw-1", :ip => "192.168.100.17" },
  { :name => "test-gw-2", :ip => "192.168.100.33" }
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  HOSTS.each do |host|
    config.vm.define host[:name] do |h|
      h.vm.box = "centos/7"
      h.vm.network "private_network", ip: host[:ip]
      h.vm.provider :virtualbox do |v|
        v.memory = 512
        v.cpus = 1
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
      end
      if host[:name] == HOSTS.last[:name]
        config.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.verbose = "v"
          ansible.playbook = "playbooks/gateway.yaml"
          ansible.groups = {
            "gateways" => HOSTS.map{|h| h[:name]}
          }
        end
      end
    end
  end
end
