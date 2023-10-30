# Instalacion-ownCloud
Como instalar ownCloud utilizando vagrant

1. smx2b@turing-117 mkdir manualowncloud
2. smx2b@turing-117 cd manualowncloud
3. smx2b@turing-117 vagrant init ubuntu/jammy64
4. smx2b@turing-117 vagrant up --provider=virtualbox
5. smx2b@turing-117 vagrant ssh
# Convertimos en root 
6. sudo -s
7. apt update
8. apt upgrade
# Ahora instalamos el apache, mysql y php 
9. apt install -y apache2
10. apt install -y mysql-server
11. apt install -y php libapache2-mod-php
12. apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
# Ahora reiniciamos el servidor apache
13. systemctl restart apache2
# Ahora configuramos el mysql
14. smx2b@turing-117 mysql

CREATE DATABASE bbdd;

CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

GRANT ALL ON bbdd.* to 'usuario'@'localhost';

# Salimos

exit
# Comprobamos conexion desde afuera
15. smx2b@turing-117 mysql -u usuario -p 
# Ahora descargamos el zip de owncloud desde este link

https://owncloud.com/download-server/

# Una vez descargado movemos el archivo a la carpeta donde tenemos nuestro repositorio

Despues movemos el archvio desde vagrant a /var/www/html

mv /vagrant/owncloud.zip /var/www/html/

# Hacemos cd a /var/www/html y ponemos estos comandos

chmod -R 775 .
chown -R root:www-data .

# Le hacemos unzip al archvivo

unzip owncloud.zip

# Eliminamos los demas archivos

rm owncloud.zip
rm index.html

# Ahora eliminamos los php que queremos

apt-get remove php-common

# Ahora instalamos unos nuevos

add-apt-repository ppa:ondrej/php -y

apt install php7.4 php7.4-intl php7.4-mysql php7.4-mbstring \
       php7.4-imagick php7.4-igbinary php7.4-gmp php7.4-bcmath \
       php7.4-curl php7.4-gd php7.4-zip php7.4-imap php7.4-ldap \
       php7.4-bz2 php7.4-ssh2 php7.4-common php7.4-json \
       php7.4-xml php7.4-dev php7.4-apcu php7.4-redis \
       libsmbclient-dev php-pear php-phpseclib

# Ahora ponemos este comando y escogemos el numero 2

update-alternatives --config php

# Ahora vamos a la carpeta de vagrant y hacemos vi

vi vagrantfile

# Y los modificamos

# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
# NOTE: This will enable public access to the opened port
config.vm.network "forwarded_port", guest: 80, host: 8080

# Create a public network, which generally matched to bridged network.
# Bridged networks make the machine appear as another physical device on
# your network.

config.vm.network "public_network"

#Salimos

exit

# Apagamos

vagrant halt

# Encendemos

vagrant up --provider=virtualbox

# Y hacemos ssh

vagrant ssh
