require 'set'

# Get env files and example files
env_list = Dir['**/.env*'].group_by { |fn| fn.end_with?('.example') }
examples = env_list[true]
envs = env_list[false]

# Abort if files do not exist
no_examples = envs.map { |fn| fn + '.example' } - examples
no_envs = examples.map { |fn| fn[0...-'.example'.size] } - envs
no_files = no_examples + no_envs
abort "Required: #{no_files}" unless no_files.empty?

# Abort if files have different contents
differences = envs.map { |fn| [fn, fn + '.example'] }.reject do |env,example|
  env_vars = File.read(env).scan(/^(.+?)=/)
  example_vars = File.read(example).scan(/^(.+?)=/)
  env_vars.to_set == example_vars.to_set
end
abort "Different: #{differences}" unless differences.empty?

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
    config.vm.hostname = "log"

    config.vm.network "public_network"

    config.vm.network "forwarded_port", guest: 80, host: ENV['LOG_PORT_80']
    config.vm.network "forwarded_port", guest: 24284, host: ENV['LOG_PORT_24284']

    # ssh settings
    config.vm.network "forwarded_port", guest: 22, host: ENV['LOG_PORT_22']
    config.ssh.port = ENV['LOG_PORT_22']

    config.vm.provision :shell, inline: <<-EOC
      cd /vagrant/log
      ./mkdirs.sh
      docker-compose pull
      docker-compose stop
      docker-compose rm -f
      docker-compose up -d
    EOC

    config.vm.provider "virtualbox" do |vb|
      vb.cpus = ENV['LOG_CPUS']
      vb.memory = ENV['LOG_MEMORY']
    end
  end

  config.vm.define :service do |config|
    config.vm.hostname = "service"

    config.vm.network "public_network"

    config.vm.network "forwarded_port", guest: 80, host: ENV['SERVICE_PORT_80']

    # ssh settings
    config.vm.network "forwarded_port", guest: 22, host: ENV['SERVICE_PORT_22']
    config.ssh.port = ENV['SERVICE_PORT_22']

    config.vm.provision :shell, inline: <<-EOC
      cd /vagrant/service
      ./mkdirs.sh
      docker-compose pull
      docker-compose stop
      docker-compose rm -f
      docker-compose up -d
    EOC

    config.vm.provider "virtualbox" do |vb|
      vb.cpus = 1
      vb.memory = 512
    end
  end
end
