# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
 config.vm.provision "shell", inline: <<-SHELL
yum install -y wget
wget https://opscode-omnibus-packages.s3.amazonaws.com/el/7/x86_64/chefdk-0.10.0-1.el7.x86_64.rpm
rpm -i chefdk-0.10.0-1.el7.x86_64.rpm
if [ ! -d /var/chef/cache/ ]; then mkdir -p /var/chef/cache/; fi
if [ ! -d /var/chef/cookbooks/ ]; then mkdir -p /var/chef/cookbooks/; fi
if [ ! -d /var/chef/checksums/ ]; then mkdir -p /var/chef/checksums/; fi
if [ ! -d /etc/chef ]; then mkdir -p /etc/chef; fi
if [ ! -d /var/chef/backup ]; then mkdir -p /var/chef/backup; fi
if [ ! -d /var/chef/environments ]; then mkdir -p /var/chef/environments; fi
if [ ! -f /var/chef/environments/production.rb ]; then touch /var/chef/environments/production.rb; fi
cat << EOF > /etc/chef/solo.rb
require 'socket'
checksum_path '/var/chef/checksums'
cookbook_path '/var/chef/cookbooks'
environment 'production'
environment_path '/var/chef/environments'
file_backup_path '/var/chef/backup'
file_cache_path '/var/chef/cache'
log_level :info
log_location STDOUT
node_name Socket.gethostname
EOF
cat << EOF > /etc/chef/solo.json
{
  "run_list": [
    "recipe[your-cookbook::default]"
  ]
}
EOF
yum install -y git
cd /var/chef/cookbooks
git clone https://github.com/rtacconi/testhome_cookbook.git testhome
cd testhome
berks
chef-solo -c /etc/chef/solo.rb -o testhome
 SHELL
end
