$install_packages = <<INSTALLER
sudo yum -y update
sudo yum install -y centos-release-openshift-origin311 dnsmasq
sudo yum install -y openshift-ansible
INSTALLER


Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"

  config.vm.provider "virtualbox" do |v, override|
    v.memory = 4096
    v.cpus   = 2
    v.customize ["modifyvm", :id, "--cpus", "2"]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.provider "libvirt" do |v, override|
    v.driver = "kvm"
    v.memory = 4096
    v.cpus   = 2
    v.suspend_mode = "managedsave"
    v.storage_pool_name = "images"
    v.machine_virtual_size = 60
  end

  config.trigger.after :up do |trigger|
     trigger.info = "OpenShift installed."
     trigger.run = {path: "get-info"}
  end


  config.vm.network :private_network, :libvirt__network_name => "default"

  config.vm.provision "shell", inline: $install_packages
  config.vm.provision "shell", path: "setup-dnsmasq", privileged: true
  config.vm.provision "shell", inline: "systemctl restart dnsmasq", privileged: true

  config.vm.provision "file", source: "ansible-hosts", destination: "ansible-hosts"
  config.vm.provision "shell", inline: "cp ansible-hosts /etc/ansible/hosts", privileged: true
  config.vm.provision "shell", path: "patch-hosts", privileged: true

  config.vm.provision "shell", inline: "rm -f /root/.ssh/id_rsa*", privileged: true
  config.vm.provision "shell", inline: "ssh-keygen -q -t rsa -N '' -f /root/.ssh/id_rsa", privileged: true
  config.vm.provision "shell", inline: "cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys", privileged: true

  config.vm.provision "shell", inline: "cd /usr/share/ansible/openshift-ansible; sed -i 's/python-docker/python-docker-py/' playbooks/init/base_packages.yml", privileged: true
  config.vm.provision "shell", inline: "cd /usr/share/ansible/openshift-ansible; ansible-playbook playbooks/prerequisites.yml", privileged: true
  config.vm.provision "shell", inline: "cd /usr/share/ansible/openshift-ansible; ansible-playbook playbooks/deploy_cluster.yml", privileged: true

  config.vm.provision "shell", path: "create-os-user", privileged: true
end


