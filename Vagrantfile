# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  is_windows = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)
  config.vm.synced_folder ".",          "/vagrant", nfs: !is_windows
  config.bindfs.bind_folder "/vagrant", "/vagrant", :owner => "vagrant", :group => "vagrant"
  config.vm.synced_folder "~/.ssh",     "/vagrant/ssh", nfs: !is_windows
  config.bindfs.bind_folder "/vagrant/ssh", "/home/vagrant/ssh", :owner => "vagrant", :group => "vagrant"

  # for nfs setting
  config.vm.network :private_network, ip: "192.168.33.10"

#  config.vm.provision "ansible" do |ansible|
#    ansible.playbook = ENV['ANSIBLE_PLAYBOOK'] || "site.yml"
#    ansible.sudo = true
#    ansible.tags = ENV['ANSIBLE_TAGS']
#    ansible.skip_tags = ENV['ANSIBLE_SKIP_TAGS']
#    ansible.groups = {
#      "api-servers" => %(dev),
#      "admin-servers" => %(dev),
#      "db-servers" => %(dev),
#      "worker-servers" => %(dev),
#      "imgproxy-servers" => %(dev),
#      "development:children" => ['api-servers', 'admin-servers', 'db-servers', 'worker-servers', 'imgproxy-servers'],
#    }
#  end

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    # IPv6とDNSでのネットワーク遅延対策で追記
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]

    # ホスト側と時刻の同期
    vb.customize ["setextradata", :id, "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled", 0]
  end
end
