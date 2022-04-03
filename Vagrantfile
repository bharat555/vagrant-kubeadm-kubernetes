Vagrant.configure("2") do |config|

  if Vagrant.has_plugin?("vagrant-vbguest") then
          config.vbguest.auto_update = false
  end
  config.vm.provision "shell", inline: <<-SHELL
      echo "10.0.0.10  master-node" >> /etc/hosts
      echo "10.0.0.11  worker-node01" >> /etc/hosts
      echo "10.0.0.12  worker-node02" >> /etc/hosts
  SHELL

  config.vm.box = "centos/stream8"
  config.vm.box_version = "20210210.0"
  config.vm.box_check_update = true

  config.vm.define "master" do |master|
    master.vm.hostname = "master-node"
    master.vm.network "private_network", ip: "10.0.0.10"
    master.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end
    master.vm.provision "shell", path: "scripts/common.sh"
    master.vm.provision "shell", path: "scripts/master.sh"
  end

  (1..2).each do |i|

  config.vm.define "node0#{i}" do |node|
    node.vm.hostname = "worker-node0#{i}"
    node.vm.network "private_network", ip: "10.0.0.1#{i}"
    node.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.cpus = 1
    end
    node.vm.provision "shell", path: "scripts/common.sh"
    node.vm.provision "shell", path: "scripts/node.sh"
  end

  end
end 
