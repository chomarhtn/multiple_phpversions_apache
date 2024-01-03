# multiple_phpversions_apache

sudo apt install software-properties-common

sudo add-apt-repository ppa:ondrej/php

sudo apt update

sudo apt-get install php7.2 php7.2-fpm php7.2-mysql libapache2-mod-php7.2 libapache2-mod-fcgid -y

sudo apt-get install php7.3 php7.3-fpm php7.3-mysql libapache2-mod-php7.3 -y

sudo systemctl start php7.2-fpm

sudo systemctl status php7.2-fpm

sudo systemctl start php7.3-fpm

sudo systemctl status php7.3-fpm

# # apache မှာ fcgid နဲ့ php versions တွေ run တယ်

sudo a2enmod actions fcgid alias proxy_fcgi  ( # apache မှာ fcgid နဲ့ php versions တွေ run တယ်)

sudo systemctl restart apache2

sudo mkdir /var/www/site1.your_domain

sudo mkdir /var/www/site2.your_domain

sudo chown -R www-data:www-data /var/www/site1.your_domain

sudo chown -R www-data:www-data /var/www/site2.your_domain

sudo chmod -R 755 /var/www/site1.your_domain

sudo chmod -R 755 /var/www/site2.your_domain

sudo nano /var/www/site1.your_domain/info.php   {<?php phpinfo(); ?>}

sudo nano /var/www/site2.your_domain/info.php   {<?php phpinfo(); ?>}

sudo nano /etc/apache2/sites-available/site1.your_domain.conf

[
<VirtualHost *:80>

     ServerName php74.chomar.linndevhouse.xyz
     
     DocumentRoot /var/www/php74/
     
     DirectoryIndex info.php

     <Directory /var/www/php74>
     
        Options Indexes FollowSymLinks MultiViews
        
        AllowOverride All
        
        Order allow,deny
        
        allow from all
        
     </Directory>

<FilesMatch \.php$>

         SetHandler "proxy:unix:/run/php/php7.4-fpm.sock|fcgi://localhost"
         
    </FilesMatch>
    
        ErrorLog ${APACHE_LOG_DIR}/site1.your_domain_error.log
        
        CustomLog ${APACHE_LOG_DIR}/site1.your_domain_access.log combined
        
</VirtualHost>

]

sudo nano /etc/apache2/sites-available/site2.your_domain.conf

[
<VirtualHost *:80>

    
     ServerName site2.your_domain
     
     DocumentRoot /var/www/site2.your_domain
     
     DirectoryIndex info.php

     <Directory /var/www/site2.your_domain>
     
        Options Indexes FollowSymLinks MultiViews
        
        AllowOverride All
        
        Order allow,deny
        
        allow from all
        
     </Directory>

    <FilesMatch \.php$>
    
         SetHandler "proxy:unix:/run/php/php7.3-fpm.sock|fcgi://localhost"
         
    </FilesMatch>

     ErrorLog ${APACHE_LOG_DIR}/site2.your_domain_error.log
     
     CustomLog ${APACHE_LOG_DIR}/site2.your_domain_access.log combined
     
</VirtualHost>
]

sudo apachectl configtest

sudo a2ensite site1.your_domain

sudo a2ensite site2.your_domain

sudo a2dissite 000-default.conf

sudo systemctl restart apache2

http://site1.your_domain 

http://site2.your_domain
