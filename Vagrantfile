# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'ubuntu1404'
  config.vm.box_url = 'https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box'
  config.vm.network :private_network, type: :dhcp
  config.vm.network :forwarded_port, guest: 9200, host: 9200
  config.vm.network :forwarded_port, guest: 5601, host: 5601

  config.vm.provider 'virtualbox' do |v|
    v.memory = 2048
  end

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'setup.yml'
  end
end
