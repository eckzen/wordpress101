#Development Environment on elementary OS 

PHP

    sudo apt-get install phpmyadmin
    select apache2

    sudo subl /etc/apache2/apache2.conf

This command will open a file on sublime text.  
Now at the very bottom of the file lets include these lines.

    Include /etc/phpmyadmin/apache.conf

Restart Apache

    sudo service apache2 restart

Open PHPMyAdmin

     http://localhost/phpmyadmin

Install

    sudo apt-get install curl libcurl3 libcurl3-dev php5-curl

###File Permission Problem

    sudo subl /etc/apache2/apache2.conf

This command will open apache2.conf file inside your text editor. Search for  <Directory /var/www> in the file. Replace /var/www with your desired PHP home folder. 

Restart Apache

    sudo service apache2 restart