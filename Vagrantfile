
  Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    public_key = File.read("id_rsa.pub")
    config.vm.provision :shell, :inline =>"
     mkdir -p /home/vagrant/.ssh
     chmod 700 /home/vagrant/.ssh
     echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
     chmod -R 600 /home/vagrant/.ssh/authorized_keys
     echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
     echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
     echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
     chmod -R 600 /home/vagrant/.ssh/config
     ", privileged: false
  
    config.vm.define "node" do |node|
        node.vm.provision "shell", inline: "sudo apt-get update && yes | sudo apt-get install apache2"
        node.vm.provision "shell", inline: "yes | sudo apt-get install php libapache2-mod-php" 
        node.vm.synced_folder "/Users/yasithaherath/Documents/DevOps/php/upload-images", "/var/www/html/upload-images/", create: true
        node.vm.hostname = "phpvm"
        node.vm.network "private_network", ip: "192.168.56.151"
        node.vm.network "forwarded_port", guest: 80, host: 8500
        node.vm.provider "virtualbox" do |vb|
            vb.memory = "1500"
        end
    end
  end