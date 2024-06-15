ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|
  config.vm.define "gate" do |gate|
    gate.vm.hostname = "gate"
    gate.vm.box = "ubuntu/focal64"
    gate.vm.network "public_network", bridge: "enp0s3"
    gate.vm.network "private_network", ip: "192.168.100.1", virtualbox__intnet: "inet"
    gate.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/gateway.yml"
    end
  end

  config.vm.define "web" do |web|
    web.vm.hostname = "web"
    web.vm.box = "ubuntu/focal64"
    web.vm.network "private_network", ip: "192.168.50.2", virtualbox__intnet: "inet"
  end

  (1..2).each do |i|
    config.vm.define "workstation#{i}" do |ws|
      ws.vm.hostname = "workstation#{i}"
      ws.vm.box = "ubuntu/focal64"
      ws.vm.network "private_network", type: "dhcp", virtualbox__intnet: "inet"
    end
  end
end