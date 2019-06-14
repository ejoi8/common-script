# Ubuntu - digitalocean  
Check default mysql password  
`cat /root/.digitalocean_password`    

**Install PhpMyadmin**  
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04    

**Change folder/files owner**  
`chown -R www-data:www-data /var/www/html/html`

**Login to MySql**  
`sudo mysql -u root -p`    

**Create Mysql Database**  
`mysql> CREATE DATABASE wordpress;`

**Create User**  
`mysql> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress@123';`    

**Grant Privileges**  
`mysql> GRANT ALL PRIVILEGES ON database_name.* TO 'wordpress'@'localhost';` OR  
`mysql> GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'localhost';` OR  
`GRANT ALL ON *.* TO 'wordpress'@'localhost' WITH GRANT OPTION;`  
`mysql> FLUSH PRIVILEGES;`

# Import SSH PublicKey  - during Ubuntu Server Installation process
ssh-import-id s****l**c*r  

**Authorized key location**  
 ~/.ssh/  
 /etc/ssh/  
