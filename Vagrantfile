Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.ssh.forward_agent = true

  unless Vagrant.has_plugin?("vagrant-vbguest")
    puts  'The Vagrant VBGuest plugin must be install prior to building this VM (vagrant plugin install vagrant-vbguest)'
  end
  
  unless Vagrant.has_plugin?("vagrant-hostsupdater")
    puts  'The Vagrant HostsUpdater plugin must be install prior to building this VM (vagrant plugin install vagrant-hostsupdater)'
  end  
  
  # Creating sharing folder benween host and guest machine 
  config.vm.synced_folder "data", "/vagrant_data"
  
  #Add selenoid & selenoid-ui to hosts file
  config.vm.network :private_network, ip: "192.168.33.10"
  config.vm.hostname = "selenoid-ui.local"
  config.hostsupdater.aliases = ["selenoid"]
  
  #Configure vm machine 
  config.vm.provider "virtualbox" do |v|
	v.memory = 4086
	v.cpus = 4
  end
  
  config.vm.provision "Install docker bu role from galaxy", type: "ansible_local" do |ansible|
	ansible.become = true
        ansible.install_mode = 'pip3'
	ansible.playbook = "docker_install.yml" 
	ansible.galaxy_role_file = "requirements.yml"
	ansible.galaxy_roles_path = "/etc/ansible/roles"
	ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
  end
    
  config.vm.provision "Install selenoid and configure it", type: "ansible_local" do |ansible|
  	ansible.become = true
	ansible.playbook = "selenoid_install.yml"
  end
    
  config.vm.provision "Install nginx", type: "ansible_local" do |ansible|
	ansible.become = true
	ansible.playbook = "nginx_install.yml"
  end
  
end
