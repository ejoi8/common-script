# common-script

# Digitalocean
Check default mysql password  
cat /root/.digitalocean_password    

**Install PhpMyadmin**  
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04    

**Change folder/files owner** 
`chown -R www-data:www-data /var/www/html/html`

**Login to MySql**  
`sudo mysql -u root -p`    

**Create Mysql Database**  
`mysql> CREATE DATABASE piwik_db_name_here;`

**Create User**  
`mysql> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress@123';`    

**Grant Privileges**  
`mysql> GRANT ALL PRIVILEGES ON database_name.* TO 'wordpress'@'localhost';` OR  
`mysql> GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'localhost';` OR  
`GRANT ALL ON *.* TO 'wordpress'@'localhost' WITH GRANT OPTION;`  
`mysql> FLUSH PRIVILEGES;`
