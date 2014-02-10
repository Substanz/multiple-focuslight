# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$yum = <<EOF
sed -i -e 's/exclude=kernel/#exclude=kernel/g' /etc/yum.conf
yum -y update
yum -y upgrade
EOF

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "CentOS-6.5-x86_64"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.5_chef-provisionerless.box"
  config.vm.network :private_network, ip: "192.168.33.200"
  config.ssh.forward_agent = true

  config.vbguest.auto_update = false

  config.vm.provision "shell", inline: $yum

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
  
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "site.yml"
    ansible.inventory_path = "vagrant"
    ansible.verbose = "vvvv"
  end
end
