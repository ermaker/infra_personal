Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "public_network"

  # ssh settings
  config.vm.network "forwarded_port", guest: 22, host: 3333
  config.ssh.port = 3333
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["keys/private", "~/.vagrant.d/insecure_private_key"]
  config.vm.provision "file", source: "keys/public", destination: "~/.ssh/authorized_keys"
  config.vm.provision "shell", inline: <<-EOC
    sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
    sudo service ssh restart
  EOC

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = 1024 * 4
  end
end
