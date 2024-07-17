ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

BOX_NAME = 'ubuntu/jammy64'
ANSIBLE_PLAYBOOK_PATH = 'ansible/vagrant/'
PRIVATE_NETWORK = 'inet'
BRIDGE = 'enp0s3'

def create_vm(config, ip, hostname, playbook, box_name, private_network, bridge)
  config.vm.define hostname do |machine|
    machine.vm.hostname = hostname
    machine.vm.box = box_name
    machine.vm.network "private_network", ip: ip, virtualbox__intnet: private_network
    machine.vm.network "public_network", bridge: bridge
    machine.vm.provision "ansible" do |ansible|
      ansible.playbook = "#{ANSIBLE_PLAYBOOK_PATH}#{playbook}"
    end
  end
end

Vagrant.configure("2") do |config|
  create_vm(config, '192.168.100.1', 'gate', 'gateway.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  create_vm(config, '192.168.100.2', 'web', 'web.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  create_vm(config, '192.168.100.5', 'admin', 'admin.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  create_vm(config, '192.168.100.6', 'db', 'db.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  create_vm(config, '192.168.100.7', 'kafka', 'kafka.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  create_vm(config, '192.168.100.8', 'zabbix', 'zabbix.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)

  prod_ips = ['192.168.100.3', '192.168.100.4']
  prod_ips.each_with_index do |ip, i|
    create_vm(config, ip, "prod#{i + 1}", 'prod.yml', BOX_NAME, PRIVATE_NETWORK, BRIDGE)
  end
end
