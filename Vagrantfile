Vagrant.configure("2") do |config|
  config.vm.box = "CentosBox/Centos7-v7.3-Minimal"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  # Add 4GB RAM
  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--name", "ansible",
      "--memory", "4096"
    ]
  end

  config.vm.hostname = "ansible"

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "ansible.yml"
  end

end
