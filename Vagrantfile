
ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|

  servers = [ "validator", "explorer", "moc", "bootnode", "netstat" ]

  if ENV["poa_platform"] == "ubuntu"
    platform = "ubuntu/xenial64"
  elsif ENV["poa_platform"] == "centos"
    platform = "centos/7"
  else
    platform = "ubuntu/xenial64"
  end

  servers.each do |machine|
    config.vm.define machine do |node|
      node.vm.box = platform
      node.vm.hostname = machine

      node.vm.provision :ansible do |ansible|
        ansible.playbook = "site.yml"
        ansible.groups = {
          "validator" => ["validator"],
          "explorer" => ["explorer"],
          "netstat" => ["netstat"],
          "moc" => ["moc"],
          "bootnode" => ["bootnode"]
        }
      end

      node.vm.provision :shell do |shell|
        shell.path = "./tests/#{machine}.sh"
      end
    end
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 1
    vb.linked_clone = true
  end
  
  config.vm.provider "docker" do |d|
    d.image = "ubuntu:16.04"
  end

end
