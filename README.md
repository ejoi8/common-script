# Ubuntu - digitalocean  
Check default mysql password  
`cat /root/.digitalocean_password`    

**Install PhpMyadmin**  
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04    

**Install ZipArchive**  
`apt-get install zip unzip`  

**Change folder/files owner**  
`chown -R www-data:www-data /var/www/html`

**Login to MySql**  
`sudo mysql -u root -p`    

**Create Mysql Database**  
`mysql> CREATE DATABASE wordpress;`

**Create User**  
`mysql> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress@123';`    

**Grant Privileges**  
`mysql> GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost';` OR  
`mysql> GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'localhost';` OR  
`mysql> GRANT ALL ON *.* TO 'wordpress'@'localhost' WITH GRANT OPTION;` 

`mysql> FLUSH PRIVILEGES;`  

**Enable a2enmod - to use .htaccess (mod_rewrite)**  
`sudo a2enmod rewrite`  

**Add & remove new user**  
`sudo adduser USERNAME` 
`sudo userdel USERNAME`  

**Add & remove group**  
`sudo groupadd GROUPNAME`
`sudo groupdel GROUPNAME`  

**Assign & revoke user in group**  
`sudo adduser USERNAME GROUPNAME`
`sudo deluser USERNAME GROUPNAME`  

**User saved in**  
`sudo vim /etc/passwd`  

**Disable Password Auth**  
`sudo nano /etc/ssh/sshd_config` - find PasswordAuthentication no  

# Import SSH PublicKey  - during Ubuntu Server Installation process
ssh-import-id s****l**c*r  

**Authorized key location**  
 ~/.ssh/  
 /etc/ssh/  

# Group Permission
777  
7 - User Owner permission  
7 - Group owner permission  
7 - Everybody else permission  

4 - Read  
2 - Write  
1 - Execute  

PHP need execute so need execute permission (1)  

# Laravel  

**Table datatyep modified**  
https://laravel.com/docs/5.8/migrations#column-modifiers  

**Create table**  
`php artisan make:migration add_votes_to_users_table --table=users`  

**Add column to existing table**  
`php artisan migrate:make add_profile_and_bio_to_users`  
`
class AddProfileToUsers extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('profile')->nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('shop_users', function (Blueprint $table) {
            $table->dropColumn(['profile']);
        });
    }
}
 `
