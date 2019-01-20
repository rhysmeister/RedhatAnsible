inc=1
Vagrant.configure("2") do |config|
  [ "ansible1",
    "web1",
    "web2",
    "db1",
    "db2",
    "tower1"].each do |host|

      config.vm.define "#{host}" do |v|
        v.vm.box = "CentosBox/Centos7-v7.3-Minimal"
        v.vm.synced_folder ".", "/vagrant", disabled: true
        v.vm.network "private_network", ip: "192.168.44.#{100 + inc}"
        inc+=1
        v.vm.provider :virtualbox do |vb|
          vb.customize [
            "modifyvm", :id,
            "--name", "#{host}",
            "--memory", "2024"
          ]
          vb.cpus = 1
        end
        config.vm.hostname = "#{host}"
        if host.eql? "ansible1"
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
        if host.eql? "web2"
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
        if host.eql? "db2"
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
        if host.eql? "tower1"
          config.vm.network "forwarded_port", guest: 80, host: 8080
          config.vm.provision :ansible do |ansible|
            ansible.groups = {
              "ansible" => ["ansible1"],
              "web" => ["web1", "web2"],
              "db" => ["db1", "db2" ],
              "tower" => ["tower1"]
            }
            ansible.limit = "tower"
            ansible.ask_vault_pass = false
            ansible.playbook = "tower.yml"
          end
        end
      end
  end
end
