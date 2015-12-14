Vagrant.configure('2') do |config|

  #config.vm.network 'public_network', bridge: 'eno1'
  config.vm.network "private_network", type: "dhcp" #, virtualbox__intnet: true

  config.vm.define 'el7-client' do |define|
    define.vm.hostname = 'el7-client'

    define.vm.provider :virtualbox do |provider, override|
      override.vm.box = 'bento/centos-7.1'
    end
  end

  config.vm.define 'server' do |define|
    define.vm.hostname = 'server'
    define.vm.provider :virtualbox do |provider, override|
      override.vm.box = 'bento/centos-6.7'
    end
  end

  config.vm.provider :virtualbox do |provider, override|
    provider.memory = 4096
    provider.cpus = 4
  end

  script = <<-EOS
   sudo yum install -y git curl patch gcc zlib-devel perl-ExtUtils-MakeMaker curl-devel gettext
   sudo yum install -y vim tree
  EOS

  config.vm.provision "shell", inline: script

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = false
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = false
  config.hostmanager.enabled = false
  config.vm.provision :hostmanager

  config.hostmanager.ip_resolver = proc do |machine|
    result = ""
    machine.communicate.execute("ifconfig") do |type, data|
      result << data if type == :stdout
    end
    (ip = /^\s*inet .*?(\d+\.\d+\.\d+\.\d+)\s+/.match(result)) && ip[1]
  end
end
