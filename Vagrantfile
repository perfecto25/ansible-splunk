# -*- mode: ruby -*-
# vi: set ft=ruby :

cluster = {
 # "master" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024 },
  "splunk01" => { :ip => "192.168.50.11", :cpus => 1, :mem => 1024 },
  "splunk02" => { :ip => "192.168.50.12", :cpus => 1, :mem => 1024 },
}

Vagrant.configure("2") do |config|
  cluster.each do |hostname, info|
    config.vm.box = "geerlingguy/centos7"
    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        override.vm.network :private_network, ip: "#{info[:ip]}"
        override.vm.hostname = hostname
        vb.name = hostname
        vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus]]
      end # end provider

      cfg.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbooks/provision.yaml"
      end
    end # end config
  end # end cluster
end