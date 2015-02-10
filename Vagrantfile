# NOTE: This should match your path to the youcloud source
SelectySourcePath = "../../alisonalonso/selecty"

Vagrant.configure("2") do |config|
  config.vm.box = "base"
  config.vm.box_url = "hashicorp/precise64"  # "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
  #config.vm.network :forwarded_port, host: 4567, guest: 80
  config.vm.network :private_network, ip: "192.168.30.60"
  config.vm.synced_folder SelectySourcePath, "/home/vagrant/selecty"
  config.vm.provision "shell", :inline => <<'SCRIPT'

# Exporta a variavel que define o root path do selecty
echo "export SELECTY_HOME=/home/vagrant/selecty" | tee -a /etc/profile
. /etc/profile

# Add um alias para a maquina host
echo "10.0.2.2  host" | tee -a /etc/hosts

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

sudo -u postgres psql -c "ALTER USER postgres WITH PASSWORD 'postgres'"
echo "listen_addresses = '*'" | tee -a /etc/postgresql/9.1/main/postgresql.conf
echo "host all all 192.168.0.0/16 md5" | tee -a /etc/postgresql/9.1/main/pg_hba.conf
service postgresql restart

rm -rf /var/www
ln -fs /home/vagrant/selecty /var/www

# Configura os sites do apache
cp -f /vagrant/config/apache/sites-available/* /etc/apache2/sites-available/
a2ensite *.selecty.local
a2enmod rewrite

# Reinicia o apache
service apache2 reload

# Instala composer
curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Rodar composer instalando as dependências
cd $SELECTY_HOME && composer install

# Instala PHPUnit
wget https://phar.phpunit.de/phpunit.phar
chmod +x phpunit.phar
mv phpunit.phar /usr/local/bin/phpunit
phpunit --version

# Configura os .htaccess
cp .htaccess.default .htaccess
cp app/.htaccess.default app/.htaccess
cp app/cw/.htaccess.default app/cw/.htaccess

SCRIPT

end
