# NOTE: This should match your path to the youcloud source
YouCloudSourcePath = "../youcloud"

Vagrant.configure("2") do |config|
  config.vm.box = hashicorp/precise64
  config.vm.network :forwarded_port, host: 4567, guest: 80 
  config.vm.network "public_network"  
  config.vm.provision "shell", path: "create_host_file.sh", args: ENV['SHELL_ARGS']   
  config.vm.synced_folder YouCloudSourcePath, "/home/vagrant/code/youcloud", :nfs => true
end
