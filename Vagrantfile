# README
#
# Getting Started:
# 1. vagrant plugin install vagrant-hostmanager
# 2. vagrant up
# 3. vagrant ssh
#
# This should put you at the control host
#  with access, by name, to other vms
Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true

  config.vm.box = "genebean/centos-8-docker-ce"
  config.vm.box_version = "2.4.20201113"

  config.vm.define "control", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.129.10"
    h.vm.hostname = "ESBCNACSPROD001"
    h.vm.provision :shell, inline: 'yum install -y ansible'
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end

  config.vm.define "machine01" do |h|
    h.vm.network "private_network", ip: "192.168.129.100"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    h.vm.hostname = "ESBCNAASPROD001" 
  end

  config.vm.define "machine02" do |h|
    h.vm.network "private_network", ip: "192.168.129.101"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    h.vm.hostname = "ESBCNAASPROD002" 
  end

  config.vm.define "machine03" do |h|
    h.vm.network "private_network", ip: "192.168.129.102"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
    h.vm.hostname = "ESBCNAASPROD003" 
  end
end
