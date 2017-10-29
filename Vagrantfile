# -*- mode: ruby -*-
# vi: set ft=ruby :

cluster = {
 # "master" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024 },
  "splunk01" => { :ip => "192.168.50.11", :cpus => 1, :mem => 1024 },
  "splunk02" => { :ip => "192.168.50.12", :cpus => 1, :mem => 1024 },
  #"slave3" => { :ip => "192.168.33.13", :cpus => 1, :mem => 1024 }
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

      # cfg.vm.provision "file", source: "~/.ssh/id_rsa", destination: ".ssh/id_rsa"
      # cfg.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: ".ssh/id_rsa.pub"
      # cfg.vm.provision :shell do |s|
      #     ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
      #     s.inline = <<-SHELL
      #       useradd mrx
      #       mkdir /home/mrx/.ssh
      #       echo "mrx     ALL=(ALL)  NOPASSWD: ALL > /etc/sudoers.d/mrx"
      #       echo #{ssh_pub_key} >> /home/mrx/.ssh/authorized_keys
      #       echo #{ssh_pub_key} >> /home/mrx/.ssh/id_rsa.pub
      #     SHELL
      # end
      
      
        # cfg.vm.synced_folder "playbooks", "/app"
    end # end config
  end # end cluster
  

  
  #config.ssh.insert_key = false
end