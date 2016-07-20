# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

   if Vagrant.has_plugin?("vagrant-cachier")

      # Configure cached packages to be shared between instances of the same base box.
      # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
        config.cache.scope = :box

      # OPTIONAL: If you are using VirtualBox, you might want to use that to enable
      # NFS for shared folders. This is also very useful for vagrant-libvirt if you
      # want bi-directional sync
      config.cache.synced_folder_opts = {
        type: :rsync,
        # The nolock option can be useful for an NFSv3 client that wants to avoid the
        # NLM sideband protocol. Without this option, apt-get might hang if it tries
        # to lock files needed for /var/cache/* operations. All of this can be avoided
        # by using NFSv4 everywhere. Please note that the tcp option is not the default.
        mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
      }
      # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
    end


  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "forwarded_port", guest: 5601, host: 5601
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 5001, host: 5001
  config.vm.network "forwarded_port", guest: 5002, host: 5002
  config.vm.network "forwarded_port", guest: 6000, host: 6000
  config.vm.network "forwarded_port", guest: 6001, host: 6001
  config.vm.network "forwarded_port", guest: 6002, host: 6002
  config.vm.network "forwarded_port", guest: 9200, host: 9200
  config.vm.network "forwarded_port", guest: 4546, host: 4546
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8090, host: 8090
  config.vm.network "forwarded_port", guest: 9000, host: 9000
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 81, host: 8180

  HOSTNAME = 'docker'
  DOMAIN   = 'elkstack.local'
  Vagrant.require_version '>= 1.7.0'
  config.ssh.insert_key = false

config.vm.host_name = HOSTNAME + '.' + DOMAIN

config.vm.provider "virtualbox" do |v|
  v.memory = 3000
  v.cpus = 2
end

config.vm.provider "vmware_fusion" do |v|
  v.vmx["memsize"] = "2024"
  v.vmx["numvcpus"] = "2"
end

$script = <<SCRIPT
sudo apt-get -y update
sudo apt-get -y dist-upgrade
echo "=> Installing docker-engine ..."
curl -sSL https://get.docker.com/ | sh  > /dev/null 2>&1
echo "=> Configuring vagrant user ..."
sudo usermod -aG docker vagrant
sudo usermod -aG docker ubuntu
sudo sysctl -w vm.max_map_count=262144

echo "=> Installing docker-compose ..."
sudo sh -c "curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose-`uname -s`-`uname -m` >/usr/local/bin/docker-compose 2>/dev/null"
sudo chmod +x /usr/local/bin/docker-compose > /dev/null 2>&1
echo "=> Finished installation of Docker"
sudo apt-get -y install openssl
SCRIPT

config.vm.provision "shell" , inline: "sudo sysctl -w vm.max_map_count=262144", run: "always"

config.vm.provision "shell",
    inline: $script
end

