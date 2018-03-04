# drupal-simple
1. Create 2 security groups
   RDS-Drupal - allow access to RDS from the Drupal EC2 instance
   EC2-Drupal - allow ssh http https access

2. Launch EC2 instance, tag Drupal, select EC2-Drupal security group

3. Create subnet group, add subnets

4. Create RDS Instance, select RDS-Drupal security group

5. ssh into Drupal EC2 instance.

6. Execute following commands

sudo su
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get autoremove -y
apt-get install apache2 php5 php5-cli php5-fpm php5-gd libssh2-php libapache2-mod-php5 php5-mcrypt php5-mysql git unzip zip postfix php5-curl mailutils php5-json -y
a2enmod rewrite headers
php5enmod mcrypt

Ubuntu specific...
nano /etc/apache2/sites-enabled/000-default.conf

<VirtualHost *:80>
        #ServerName example.com
        #ServerAlias www.example.com
        DocumentRoot /var/www/drupal

        <Directory /var/www/drupal>
                Options -Indexes
                AllowOverride All
                Order allow,deny
                Allow from all
        </Directory>
</VirtualHost>

cd /var/www
wget http://ftp.drupal.org/files/projects/drupal-7.36.zip
unzip drupal-7.36.zip
mv drupal-7.36 drupal
cd drupal
chmod -R 744 .
chown -R www-data:www-data .

service apache2 restart

7. Run Drupal setup.....
   using RDS Instance for database(not localhost), remember to remove port number and add to next field.
