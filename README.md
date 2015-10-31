# Wordpress101
This is how it all started

https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-14-04

https://ch0yan.wordpress.com/2015/01/04/development-environment-on-elementary-os-ubuntu-part-2-php-installation/

###MySQL

    mysql -u root -p

Create a new database

    CREATE DATABASE wordpress;

Create a new user account

    CREATE USER eckzen@localhost IDENTIFIED BY 'password';

Grant privileges to that new user

    GRANT ALL PRIVILEGES ON wordpress.* TO eckzen@localhost;
    
    FLUSH PRIVILEGES;

    exit

###Download wordpress

    cd ~
    wget http://wordpress.org/latest.tar.gz

Extract the wordpress file

    tar xzvf latest.tar.gz

Update the local packages

    sudo apt-get update
    sudo apt-get install php5-gd libssh2-php

###Configure Wordpress

    cd ~/wordpress
    cp wp-config-sample.php wp-config.php
    subl wp-config.php

Change the fields

    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define('DB_NAME', 'wordpress');

    /** MySQL database username */
    define('DB_USER', 'wordpressuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'password');

###Copy the files to the Document Root

    sudo rsync -avP ~/wordpress/ /var/www/html/

Go to the document Root

    cd /var/www/html

Change group ownership

    sudo chown -R eckzen:www-data *

Create uploadsdirectory beneath wp-content manually

    sudo chown -R eckzen:www-data * <-- this cause an error. 
                                        Note: bcoz im stupid. it will work if i start all over 
    sudo chown -R www-data *        <-- this works

    sudo chown -R :www-data /var/www/html/wp-content/uploads

###Modifying Apache to Allow URL Rewrites

    sudo subl /etc/apache2/sites-available/000-default.conf

    <VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html
    ServerName server_domain_name_or_IP
    <Directory /var/www/html/>
        AllowOverride All
    </Directory>
    . . .

Enable rewrite module

    sudo a2enmod rewrite

Restart Apache

    sudo service apache2 restart


###Create an .htaccess File

    touch /var/www/html/.htaccess

Change the owner

    sudo chown :www-data /var/www/html/.htaccess

If you want WordPress to automatically update this file with rewrite rules, you can ensure that it has the correct permissions to do so by typing:

    sudo chmod 664 /var/www/html/.htaccess

if i want ti tighten the security

    sudo chmod 644 /var/www/html/.htaccess

###Change the permalink settings in wordpress

    Settings > Permalinks
    Select want you want
    Save Changes

###How to Increase the Maximum File Upload Size in WordPress

http://www.wpbeginner.com/wp-tutorials/how-to-increase-the-maximum-file-upload-size-in-wordpress/

.htcaccess fix

    # BEGIN WordPress
    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    </IfModule>
    php_value upload_max_filesize 64M
    php_value post_max_size 64M
    php_value max_execution_time 300
    php_value max_input_time 300
    # END WordPress

###ERROR

    Briefly unavailable for scheduled maintenance. Check back in a minute.

##fix 

    delete .maintenence file in the root