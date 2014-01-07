# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.network :forwarded_port, guest:8000, host: 8080
  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "openssl"
    chef.add_recipe "postgresql::server"

    chef.json = { 
      :postgresql => {
        :version => "8.4",
        :password => {
          :postgres => "password"
        }
      },
      :build_essential => {
        :compiletime => true
      }
    }
  end

  config.vm.provision :shell do |s|
    $script = <<-eos
      sudo -u postgres createdb development
      sudo apt-get -q -y install git-core python-virtualenv python-imaging python-dev
    eos

    s.inline = $script
  end

end
