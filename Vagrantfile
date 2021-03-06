Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provider 'virtualbox' do |v, override|
    v.memory = 1024

    override.vm.synced_folder '.', '/home/vagrant/NUSMods', type: 'nfs'

    if Vagrant.has_plugin?('vagrant-cachier')
      override.cache.scope = :machine
    end
  end

  config.vm.network 'forwarded_port', guest: 80, host: 8080, auto_correct: true
  config.vm.network 'forwarded_port', guest: 9000, host: 9000, auto_correct: true
  config.vm.network 'forwarded_port', guest: 35729, host: 35729, auto_correct: true
  config.vm.network 'private_network', type: 'dhcp'
  config.ssh.forward_agent = true
  config.vm.synced_folder '.', '/vagrant', disabled: true

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'provisioning/vagrant.yml'
    ansible.verbose = 'v'
  end

  config.vm.provider :digital_ocean do |provider, override|
    override.ssh.private_key_path = '~/.ssh/id_rsa'
    override.ssh.username = 'ubuntu'
    override.vm.box = 'digital_ocean'
    override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
    override.vm.synced_folder '.', '/home/ubuntu/NUSMods', type: 'nfs'

    provider.client_id = ENV['DIGITALOCEAN_CLIENT_ID']
    provider.api_key = ENV['DIGITALOCEAN_API_KEY']
    provider.image = 'Ubuntu 14.04 x64'
    provider.region = 'Singapore 1'
    provider.size = '2GB'
  end
end
