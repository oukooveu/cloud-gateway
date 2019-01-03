# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

HOSTS = [
  { :name => "test-gw-1", :ext_ip => "172.16.100.17", :int_name => "test-1", :int_ip => "192.168.17.1" },
  { :name => "test-host-1", :int_name => "test-1", :int_ip => "192.168.17.2", :gateway => "192.168.17.1" },
  { :name => "test-gw-2", :ext_ip => "172.16.100.33", :int_name => "test-2", :int_ip => "192.168.18.1" },
  { :name => "test-host-2", :int_name => "test-2", :int_ip => "192.168.18.2", :gateway => "192.168.18.1" }
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  HOSTS.each do |host|
    config.vm.define host[:name] do |machine|
      machine.vm.box = "centos/7"
      machine.vm.hostname = host[:name]
      machine.vm.network "private_network", ip: host[:ext_ip] if host.key?(:ext_ip)
      machine.vm.network "private_network", ip: host[:int_ip], virtualbox__intnet: host[:int_name]
      machine.vm.provider :virtualbox do |v|
        v.memory = 512
        v.cpus = 1
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
      end
      # change routing for regular hosts
      if host.has_key?(:gateway)
        machine.vm.provision "shell",
          run: "always",
          inline: "ip route add default via " + host[:gateway] + " || true"
        machine.vm.provision "shell",
          run: "always",
          inline: "eval `ip route | awk '{ if ($1 ==\"default\" && $5 == \"eth0\") print \"ip route del default via \" $3; }'`"
      end
      # setup gateways
      if host[:name] == HOSTS.last[:name]
        machine.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.verbose = "v"
          ansible.playbook = "playbooks/gateway.yaml"
          ansible.groups = {
            "gateways" => HOSTS.select{|h| h.key?(:ext_ip)}.map{|h| h[:name]}
          }
        end
      end
    end
  end
end
