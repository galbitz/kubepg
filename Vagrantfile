#
#  in the wiki:  https://github.com/josenk/vagrant-vmware-esxi/wiki

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

esxi_hostname = '192.168.1.139'
esxi_username = 'root'
esxi_password = 'P@ssword1!'

Vagrant.configure('2') do |config|

  config.vm.provision "shell", path: "bootstrap.sh"

  MasterCount = 1

  (1..MasterCount).each do |i|
    config.vm.define "kmaster#{i}" do |node|
      node.vm.box = 'generic/ubuntu2004'
      node.vm.box_check_update = false
      node.vm.hostname = "kmaster#{i}.example.com"
      node.vm.network 'public_network', ip: "192.168.1.#{i+200}"
      node.vm.synced_folder('.', '/vagrant', type: 'nfs', disabled: true)
      node.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = esxi_hostname  
        esxi.esxi_username = esxi_username
        esxi.esxi_password = esxi_password

        esxi.guest_memsize = '4096'
      end
      
      node.vm.provision "shell", path: "bootstrap_kmaster.sh"
    end
  end

  NodeCount = 2

  (1..NodeCount).each do |i|
    config.vm.define "kworker#{i}" do |node|
      node.vm.box = 'generic/ubuntu2004'
      node.vm.box_check_update = false
      node.vm.hostname = "kworker#{i}.example.com"
      node.vm.network 'public_network', ip: "192.168.1.#{i+210}"
      node.vm.synced_folder('.', '/vagrant', type: 'nfs', disabled: true)
      node.vm.provider :vmware_esxi do |esxi|
        esxi.esxi_hostname = esxi_hostname  
        esxi.esxi_username = esxi_username
        esxi.esxi_password = esxi_password

        esxi.guest_memsize = '4096'
      end

      node.vm.provision "shell", path: "bootstrap_kworker.sh"
    end    
  end


end
