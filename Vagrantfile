# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
  :box_name => "centos/7",
        :net => [
                    {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                ]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", gateway: "192.168.255.1", virtualbox__intnet: "router-net"},
                    {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "central-net"},
                    {ip: '192.168.2.1', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "office1-net"},
                    {ip: '192.168.1.1', adapter: 4, netmask: "255.255.255.128", virtualbox__intnet: "office2-net"}
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", gateway: "192.168.0.1", virtualbox__intnet: "central-net"},
                ]
  },

  :office1Router => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.2.2', adapter: 2, netmask: "255.255.255.192", gateway: "192.168.2.1", virtualbox__intnet: "office1-net"},
                    {ip: '192.168.2.65', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "office1-net"}        
                ]  
  },

  :office1Server => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.2.66', adapter: 2, netmask: "255.255.255.192", gateway: "192.168.2.65", virtualbox__intnet: "office1-net"}        
                ]  
  },

  :office2Router => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", gateway: "192.168.1.1", virtualbox__intnet: "office2-net"},
                    {ip: '192.168.1.129', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "office2-net"}
                ]  
  },

  :office2Server => {
        :box_name => "centos/7",
        :net => [
                    {ip: '192.168.1.130', adapter: 2, netmask: "255.255.255.192", gateway: "192.168.1.129", virtualbox__intnet: "office2-net"}        
                ]  
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL

        box.vm.provision "ansible" do |ansible|
          ansible.become = true
          ansible.verbose = "v"
          ansible.playbook = "provision/provision.yml"
        end 

      end

  end
  
end

