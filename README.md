# Ubuntu - digitalocean  

**install php7 module** 

GD PHP Extension: 
        
        sudo apt-get install php-gd
        
IMAP PHP Extension: 

        sudo apt-get install php-imap

MBString PHP Extension: 

        sudo apt-get install php-mbstring

cURL PHP Extension: 

        sudo apt-get install php-curl 

Zip PHP Extension: 

        sudo apt-get install php-zip 


**install lamp ubuntu 18.04**  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04  

**Check default mysql password**  

        cat /root/.digitalocean_password

**Install PhpMyadmin**  
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04    

**Install ZipArchive**  

        apt-get install zip unzip

**Change folder/files owner**  

        chown -R www-data:www-data /var/www/html

**Login to MySql**  

        sudo mysql -u root -p    

**Create Mysql Database**  

        mysql> CREATE DATABASE wordpress;

**Create User**  

        mysql> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'wordpress@123';    

**Grant Privileges**  

        mysql> GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'localhost'; OR  
        mysql> GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'localhost'; OR  
        mysql> GRANT ALL ON *.* TO 'wordpress'@'localhost' WITH GRANT OPTION; 

        mysql> FLUSH PRIVILEGES;  

**Enable a2enmod - to use .htaccess (mod_rewrite)**  

       sudo a2enmod rewrite
add:  

        <Directory /var/www/html>  
                Options Indexes FollowSymLinks MultiViews  
                AllowOverride All  
                Require all granted  
        </Directory>  

Then run:

        sudo nano /etc/apache2/sites-available/000-default.conf  

**Add & remove new user**  

        sudo adduser USERNAME
        sudo userdel USERNAME

**Add & remove group**  

        sudo groupadd GROUPNAME
        sudo groupdel GROUPNAME 

**Assign & revoke user in group**  

        sudo adduser USERNAME GROUPNAME
        sudo deluser USERNAME GROUPNAME  

**User saved in**  

        sudo vim /etc/passwd

**Disable Password Auth**  

find PasswordAuthentication no 

        sudo nano /etc/ssh/sshd_config

# Import SSH PublicKey  - during Ubuntu Server Installation process

        ssh-import-id s****l**c*r  

**Authorized key location** 

         ~/.ssh/  
         /etc/ssh/  

# Group Permission

        4 - Read  
        2 - Write  
        1 - Execute 
        
        777  
        
        how the 7 get value is 4+2+1 = 7
        
        7 - User Owner permission
        7 - Group owner permission
        7 - Everybody else permission
        
        PHP need execute so need execute permission (1)  

**LETS ENCRYPT SSL**  
https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-18-04  

# INSTALL WORDPRESS  
    $ wget -c http://wordpress.org/latest.tar.gz
    $ tar -xzvf latest.tar.gz
    $ sudo rsync -av wordpress/* /var/www/html/
    $ sudo chown -R www-data:www-data /var/www/html/
    $ sudo chmod -R 755 /var/www/html/

# Laravel  

**error message**  

        @if ($errors->any())
            <div class="row">
                <div class="col-md-12">
                    <div class="m-form__content">
                        <div class="m-alert m-alert--icon alert alert-danger" role="alert" id="m_form_1_msg">
                            <div class="m-alert__icon">
                                <i class="la la-warning"></i>
                            </div>
                            <div class="m-alert__text">
                                Oh snap! Change a few things up and try submitting again.
                                 @foreach($errors->all() as $error)
                                     <li>{{ $error }}</li>
                                @endforeach
                            </div>
                            <div class="m-alert__close">
                                <button type="button" class="close" data-close="alert" aria-label="Close">
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        @endif

**Table datatyep modified**  
https://laravel.com/docs/5.8/migrations#column-modifiers  

**Create table**  

        php artisan make:migration create_users_table --create=users 

**Add column to existing table**  

        php artisan make:migration add_votes_to_users_table --table=users
        
Inside the generated file:  

        public function up()  
        {  
                Schema::table('users', function (Blueprint $table) {  
                $table->string('profile')->nullable();  
        });  
        }  
        public function down()  
        {  
                Schema::table('users', function (Blueprint $table) {  
                $table->dropColumn(['profile']);  
        });  
        }
    
    
**SAMPLE IF SET FOREIGN KEY ON EXISTIN TABLE**  
    
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::disableForeignKeyConstraints();
        Schema::table('user', function (Blueprint $table) {
            $table->unsignedInteger('user_id')->after('user_id')->default('1');

            $table->foreign('user_id')
                    ->references('id')->on('user_category')
                    ->onDelete('cascade');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('user', function (Blueprint $table) {
            $table->dropForeign('user_user_id_foreign');
            $table->dropColumn(['user_id']);
        });
    }
 
# OPENLITESPEED - digitalocean  

**Lets encrypt SSL** 

        certbot certonly --webroot -w /var/www/html -d www.website.com 

Here are the list of **.pem** files you see under **/etc/letsencrypt/live/<yourdomain>/** directory.

**cert.pem** - SSL certificate of your domain  
**chain.pem** - CA certificate  
**fullchain.pem** - Combined certificate, includes domain and CA certificate  
**privkey.pem** - Private key  

https://www.itzgeek.com/how-tos/linux/how-to-configure-lets-encrypt-ssl-in-openlitespeed-web-server.html
https://servernesia.com/833/mengaktifkan-https-openlitespeed/

**Allow to access WebAdmin**  

        ufw allow 7080

**Block to access WebAdmin** 

        ufw deny 7080

**Get the WebAdmin admin password:**  

        cat .litespeed_password

**Get the MySQL root password:**  

        sudo sed -n 1p .db_password  

**wordpress username pass**

        sudo sed -n 2p .db_password  

# GENERAL COMMAND  

**to check current timezone**

        timedatectl 

**Change server timezone**

        timedatectl list-timezones | grep -i asia
        sudo unlink /etc/localtime
        sudo ln -s /usr/share/zoneinfo/Asia/Kuala_Lumpur /etc/localtime

**Crontab/scheduler**

        # ┌───────────── minute (0 - 59)
        # │ ┌───────────── hour (0 - 23)
        # │ │ ┌───────────── day of month (1 - 31)
        # │ │ │ ┌───────────── month (1 - 12)
        # │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
        # │ │ │ │ │                                       7 is also Sunday on some systems)
        # │ │ │ │ │
        # │ │ │ │ │
        # * * * * *  command_to_execute
        #
        # crontab time helper
        # https://crontab.guru

crontab for current user

        crontab -l

Add crontab

        crontab -e
        
**Change server time behind proxy**

        sudo date -s "$(wget -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f5-8)Z"
        
        
        
# Git  

**First setup**

        $ git config --global user.name "John Doe"
        $ git config --global user.email johndoe@example.com
        $ git config --list
        
**Initialization**  

        $ git init
        
**Add files**

        git add <filename>
        
**Commit**

        git commit -m "Commit message"
      
**Workflow to create new repo for ease of use**
        
        1. Create new repo in github & choose generate README.md
        2. Clone to your local machine useing: git clone https://github.com/xxxx
        3. Add your project inside the folder
        4. git push

        OR
        
        1. Create new repo in github & choose generate README.md
        2. git remote add <name> <url>
        3. git push <name> or git push --set-upstream <name> master (because the repo was created)




