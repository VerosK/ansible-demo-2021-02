# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    $use_nfs = false
    $ansible_mode = 'ansible_local'
else
    $use_nfs = true
    $ansible_mode = 'ansible'
end


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "vStone/centos-7.x-puppet.3.x"
  #config.vm.box = "centos/7"
  config.vm.box = "geerlingguy/centos7"

  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"


  config.vm.provider :virtualbox do |v|
    v.memory = 4096
    v.cpus = 2
    v.linked_clone = true
  end

  config.vm.define "db_host" do |box|
    box.vm.hostname = "test"
    box.vm.network :private_network, ip: "172.16.10.10"

    config.vm.network :forwarded_port, guest: 80, host: 3080

#    box.vm.provision :shell,
#      inline: "cp -vf /vagrant/provisioning/ansible.cfg ~vagrant/.ansible.cfg"

    box.vm.provision $ansible_mode do |ansible|
      ansible.playbook = "setup-site.yml"
      ansible.become = true
      ansible.verbose = true
      ansible.groups = {
         'vagrant' => "db_host",
      }
      if ENV['ANSIBLE_TAGS'] then ansible.tags = ENV['ANSIBLE_TAGS']; end
    end
  end

end
