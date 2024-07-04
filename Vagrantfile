ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

BOX_NAME = 'ubuntu/jammy64'
ANSIBLE_PLAYBOOK_PATH = 'ansible/vagrant/'
PRIVATE_NETWORK = 'inet'

GATEWAY_HOSTNAME = 'gate'
GATEWAY_PUBLIC_BRIDGE = 'enp0s3'
GATEWAY_IP = '192.168.100.1'

WEB_HOSTNAME = 'web'
WEB_IP = '192.168.100.2'

ADMIN_HOSTNAME = 'admin'
ADMIN_IP = '192.168.100.5'

PROD_HOSTNAME_PREFIX = 'prod'
prod_ips = ['192.168.100.3']

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

  prod_ips.each_with_index do |ip, i|
    config.vm.define "#{PROD_HOSTNAME_PREFIX}#{i + 1}" do |prod|
      prod.vm.hostname = "#{PROD_HOSTNAME_PREFIX}#{i + 1}"
      prod.vm.box = BOX_NAME
      prod.vm.network "private_network", ip: ip, virtualbox__intnet: PRIVATE_NETWORK
      prod.vm.provision "ansible" do |ansible|
        ansible.playbook = "#{ANSIBLE_PLAYBOOK_PATH}prod.yml"
      end
    end
  end

  config.vm.define ADMIN_HOSTNAME do |admin|
    admin.vm.hostname = ADMIN_HOSTNAME
    admin.vm.box = BOX_NAME
    admin.vm.network "private_network", ip: ADMIN_IP, virtualbox__intnet: PRIVATE_NETWORK
    admin.vm.provision "ansible" do |ansible|
      ansible.playbook = "#{ANSIBLE_PLAYBOOK_PATH}admin.yml"
    end
  end

end
