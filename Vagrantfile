inc=1
Vagrant.configure("2") do |config|
  [ "ansible1",
    "web1",
    "web2",
    "db1",
    "db2",
    "tower1"].each do |host|
      config.vm.synced_folder ".", "/vagrant", disabled: true
      config.vm.network "private_network", ip: "192.168.44.#{100 + inc}"
      inc+=1
      config.vm.define "#{host}" do |v|
        v.vm.box = "CentosBox/Centos7-v7.3-Minimal"
        v.vm.provider :virtualbox do |vb|
          vb.customize [
            "modifyvm", :id,
            "--name", "#{host}",
            "--memory", "2024"
          ]
          vb.cpus = 1
        end
        config.vm.hostname = "#{host}"
        if host == "ansible"
          config.vm.provision :ansible do |ansible|
            ansible.groups = {
                "ansible" => ["ansible1"],
                "web" => ["web1", "web2"],
                "db" => ["db1", "db2" ],
                "tower" => ["tower1"]
            }
            ansible.limit = "ansible"
            ansible.playbook = "ansible.yml"
          end
        end
        if host.start_with?("web")
          config.vm.provision :ansible do |ansible|
            ansible.groups = {
              "ansible" => ["ansible1"],
              "web" => ["web1", "web2"],
              "db" => ["db1", "db2" ],
              "tower" => ["tower1"]
            }
            ansible.limit = "web"
            ansible.playbook = "web.yml"
          end
        end
        if host.start_with?("db")
          config.vm.provision :ansible do |ansible|
            ansible.groups = {
              "ansible" => ["ansible1"],
              "web" => ["web1", "web2"],
              "db" => ["db1", "db2" ],
              "tower" => ["tower1"]
            }
            ansible.limit = "db"
            ansible.ask_vault_pass = true
            ansible.playbook = "db.yml"
          end
        end
        if host == "tower"
          config.vm.provision :ansible do |ansible|
            ansible.groups = {
              "ansible" => ["ansible1"],
              "web" => ["web1", "web2"],
              "db" => ["db1", "db2" ],
              "tower" => ["tower1"]
            }
            ansible.limit = "tower"
            ansible.playbook = "tower.yml"
          end
        end
      end
  end
end
