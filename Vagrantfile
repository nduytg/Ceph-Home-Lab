nodes = [
  { :hostname => 'ansible', :ip => '192.168.0.40', :ip_pub => '192.168.1.40', :box => 'xenial64' },
  { :hostname => 'osd1',  :ip => '192.168.0.51', :ip_pub => '192.168.1.51', :box => 'xenial64', :ram => 1024, :osd => 'yes' },
  { :hostname => 'osd2',  :ip => '192.168.0.52', :ip_pub => '192.168.1.52', :box => 'xenial64' , :ram => 1024, :osd => 'yes' },
  { :hostname => 'osd3',  :ip => '192.168.0.53', :ip_pub => '192.168.1.53', :box => 'xenial64' , :ram => 1024, :osd => 'yes' }
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = "bento/ubuntu-16.04"
      nodeconfig.vm.hostname = node[:hostname]
      nodeconfig.vm.network :private_network, ip: node[:ip]
      nodeconfig.vm.network :private_network, ip: node[:ip_pub]

      memory = node[:ram] ? node[:ram] : 512;
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--memory", memory.to_s,
        ]
        if node[:osd] == "yes"
            #/dev/sdb
            vb.customize [ "createhd", "--filename", "disk_osd-sdb-#{node[:hostname]}", "--size", "5000" ]
            vb.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 3, "--device", 0, "--type", "hdd", "--medium", "disk_osd-sdb-#{node[:hostname]}.vdi" ]

            #/dev/sdc
            vb.customize [ "createhd", "--filename", "disk_osd-sdc-#{node[:hostname]}", "--size", "5000" ]
            vb.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 4, "--device", 0, "--type", "hdd", "--medium", "disk_osd-sdc-#{node[:hostname]}.vdi" ]

            #/dev/sdd
            vb.customize [ "createhd", "--filename", "disk_osd-sdd-#{node[:hostname]}", "--size", "5000" ]
            vb.customize [ "storageattach", :id, "--storagectl", "SATA Controller", "--port", 5, "--device", 0, "--type", "hdd", "--medium", "disk_osd-sdd-#{node[:hostname]}.vdi" ]
        end
      end
    end
    config.hostmanager.enabled = true
    config.hostmanager.manage_guest = true
  end
end