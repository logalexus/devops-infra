ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|
  config.vm.define "gateway" do |gateway|
    gateway.vm.box = "ubuntu/bionic64"
    gateway.vm.network "public_network"
    gateway.vm.network "private_network", ip: "192.168.100.1"
    gateway.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/gateway.yml"
      ansible.groups = {
        "gateway" => ["gateway"]
      }
    end
  end

  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/bionic64"
    web.vm.network "private_network", ip: "192.168.50.2"
    web.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/web.yml"
      ansible.groups = {
        "web" => ["web"]
      }
    end
  end

  (1..2).each do |i|
    config.vm.define "workstation#{i}" do |ws|
      ws.vm.box = "ubuntu/bionic64"
      ws.vm.network "private_network", type: "dhcp"
    end
  end


  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end
end