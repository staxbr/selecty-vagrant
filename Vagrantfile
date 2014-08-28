# NOTE: This should match your path to the youcloud source
SelectySourcePath = "../selecty"

Vagrant.configure("2") do |config|
  config.vm.box = "base"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
  #config.vm.network :forwarded_port, host: 4567, guest: 80
  config.vm.network :private_network, ip: "192.168.30.60"
  config.vm.synced_folder SelectySourcePath, "/home/vagrant/selecty", :nfs => true
  config.vm.provision "shell", :inline => <<'SCRIPT'

echo "Checking System Update..."
apt-get -y update > /dev/null
echo "Update completed"

echo "Installing Apache..."
apt-get -y install apache2 > /dev/null 
echo "Apache installed"

echo "Instalando PHP..."
apt-get -y install php5 php5-gd php5-curl php5-pgsql php5-mcrypt > /dev/null
echo "PHP instalado"

echo "Installing PostgreSQL..."
apt-get -y install postgresql postgresql-plperl > /dev/null 
echo "PostgreSQL installed"

rm -rf /var/www
ln -fs /home/vagrant/selecty /var/www


SCRIPT

end
