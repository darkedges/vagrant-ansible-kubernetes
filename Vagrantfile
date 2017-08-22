# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.synced_folder_opts = {
      type: :nfs,
      mount_options: ['rw', 'vers=3', 'udp', 'nolock', 'noatime']
    }
  end
  config.vbguest.auto_update = true
  
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guests = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.provision "shell", inline: "HOSTNAME=`hostname`; sudo sed -ri \"/127\.0\.0\.1.*$HOSTNAME.*/d\" /etc/hosts"

  num_Nodes = (ENV['NUM_PF_NODES'] || 3).to_i

  num_Nodes.times do |n|

    node_index = n+1

    config.vm.define "kubernetes-0#{node_index}" , autostart: true, primary: true do |kubernetes|
      kubernetes.vm.box = "centos/7"
      kubernetes.vm.hostname = "kubernetes-0#{node_index}.local.darkedges.com"
      kubernetes.vm.network :private_network, ip: "192.168.50.#{100+node_index}"
      kubernetes.vm.hostname = "kubernetes-0#{node_index}.local.darkedges.com"
      kubernetes.hostmanager.aliases = [ "kubernetes-0#{node_index}" ]
      kubernetes.vm.synced_folder ".", "/shared", type: "nfs" 

      kubernetes.vm.provider  "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--name", "kubernetes-0#{node_index}"]
        vb.customize ["modifyvm", :id, "--cpus", "1"]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]
        vb.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
        vb.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]
      end
      
    end
  
  end

end
