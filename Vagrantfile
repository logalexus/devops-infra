ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

BOX_NAME = 'ubuntu/jammy64'
ANSIBLE_PLAYBOOK_PATH = 'ansible/'
PRIVATE_NETWORK = 'inet'

GATEWAY_HOSTNAME = 'gate'
GATEWAY_PUBLIC_BRIDGE = 'enp0s3'
GATEWAY_IP = '192.168.100.1'

WEB_HOSTNAME = 'web'
WEB_IP = '192.168.100.2'

WORKSTATION_HOSTNAME_PREFIX = 'workstation'
WORKSTATION_COUNT = 0


Vagrant.configure("2") do |config|
  config.vm.define GATEWAY_HOSTNAME do |gate|
    gate.vm.hostname = GATEWAY_HOSTNAME
    gate.vm.box = BOX_NAME
    gate.vm.network "private_network", ip: GATEWAY_IP, virtualbox__intnet: PRIVATE_NETWORK
    gate.vm.provision "ansible" do |ansible|
      ansible.playbook = "#{ANSIBLE_PLAYBOOK_PATH}gateway.yml"
    end
  end

  config.vm.define WEB_HOSTNAME do |web|
    web.vm.hostname = WEB_HOSTNAME
    web.vm.box = BOX_NAME
    web.vm.network "private_network", ip: WEB_IP, virtualbox__intnet: PRIVATE_NETWORK
    web.vm.provision "ansible" do |ansible|
      ansible.playbook = "#{ANSIBLE_PLAYBOOK_PATH}web.yml"
    end
  end

  (1..WORKSTATION_COUNT).each do |i|
    config.vm.define "#{WORKSTATION_HOSTNAME_PREFIX}#{i}" do |ws|
      ws.vm.hostname = "#{WORKSTATION_HOSTNAME_PREFIX}#{i}"
      ws.vm.box = BOX_NAME
      ws.vm.network "private_network", type: "dhcp", virtualbox__intnet: PRIVATE_NETWORK
    end
  end
end
