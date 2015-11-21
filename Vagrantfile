Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  # ssh settings
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["keys/private", "~/.vagrant.d/insecure_private_key"]
  config.vm.provision :file, source: "keys/public", destination: "~/.ssh/authorized_keys"
  config.vm.provision :shell, inline: <<-EOC
    sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
    sudo service ssh restart
  EOC

  # Install docker
  config.vm.provision :docker

  # Install docker-compose
  config.vm.provision :shell, inline: <<-EOC
    test -e /usr/local/bin/docker-compose || \\
    curl -sSL https://github.com/docker/compose/releases/download/1.5.1/docker-compose-`uname -s`-`uname -m` \\
      | sudo tee /usr/local/bin/docker-compose > /dev/null
    sudo chmod +x /usr/local/bin/docker-compose
    test -e /etc/bash_completion.d/docker-compose || \\
    curl -sSL https://raw.githubusercontent.com/docker/compose/$(docker-compose --version | awk 'NR==1{print $NF}')/contrib/completion/bash/docker-compose \\
      | sudo tee /etc/bash_completion.d/docker-compose > /dev/null
  EOC

  config.vm.define :log do |config|
    config.vm.network "public_network"

    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network "forwarded_port", guest: 24284, host: 24284

    # ssh settings
    config.vm.network "forwarded_port", guest: 22, host: 3333
    config.ssh.port = 3333

    config.vm.provision :shell, inline: <<-EOC
      cd /vagrant/log
      ./mkdirs.sh
      docker-compose pull
      docker-compose up -d
    EOC

    config.vm.provider "virtualbox" do |vb|
      vb.cpus = 2
      vb.memory = 1024 * 4
    end
  end

  config.vm.define :service do |config|
    config.vm.network "public_network"

    config.vm.network "forwarded_port", guest: 80, host: 80

    # ssh settings
    config.vm.network "forwarded_port", guest: 22, host: 3334
    config.ssh.port = 3334

    config.vm.provision :shell, inline: <<-EOC
      cd /vagrant/service
      ./mkdirs.sh
      docker-compose pull
      docker-compose up -d
    EOC

    config.vm.provider "virtualbox" do |vb|
      vb.cpus = 1
      vb.memory = 512
    end
  end
end
