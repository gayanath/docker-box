# TODO: Install Ansible only if it doesn't exisit
Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "192.168.130.10"
    config.vm.hostname = "docker"
    config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
    end
    # Following two shell privisioners are required because of a bug exisits in
    # Vagrant 1.8.1. Should be possible to remove with 1.8.2
    config.vm.provision "shell", inline: <<-SHELL
        sudo apt-get install software-properties-common
        sudo apt-add-repository ppa:ansible/ansible
        sudo apt-get -qq update
        sudo apt-get install -y ansible
    SHELL
    # Patch: https://github.com/mitchellh/vagrant/issues/6793#issuecomment-172408346
    config.vm.provision "shell" do |s|
        s.inline = '[[ ! -f $1 ]] || grep -F -q "$2" $1 || sed -i "/__main__/a \\    $2" $1'
        s.args = ['/usr/bin/ansible-galaxy', "if sys.argv == ['/usr/bin/ansible-galaxy', '--help']: sys.argv.insert(1, 'info')"]
    end
    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "setup/playbook.yml"
    end
end
