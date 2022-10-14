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
	
Copy folder with all content

	sudo cp -av dir_to_copy dir_new_name


**install lamp ubuntu 18.04**  
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04  

**Check default mysql password**  

        cat /root/.digitalocean_password

**Install PhpMyadmin**  
https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-18-04    

**Install ZipArchive**  

        apt-get install zip unzip
        
        Compress dir
        tar -zcvf file.tar.gz /path/to/dir/

        Compress multiple dir
        tar -zcvf file.tar.gz dir1 dir2 dir3
        
        Extract
        tar -xzvf file.tar.gz
	
	unzip all zip files in directory
	for z in *.zip; do unzip "$z"; done

        https://www.cyberciti.biz/faq/how-to-tar-a-file-in-linux-using-command-line/

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
        
**Update Privileges**

        UPDATE mysql.user SET Host='%' WHERE Host='localhost' AND User='username';
        UPDATE mysql.db SET Host='%' WHERE Host='localhost' AND User='username';
        FLUSH PRIVILEGES;
        
**Add user allow all hosts**

        CREATE USER 'admin'@'%' IDENTIFIED BY 'admin@123';
        GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%';
        SHOW GRANTS FOR 'admin'@'%';
        FLUSH PRIVILEGES;
        
**Allow MYSQL Remote using client

        https://www.digitalocean.com/community/tutorials/how-to-allow-remote-access-to-mysql
        
**Disable ONLY_FULL_GROUP_BY**

        SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
        SET sql_mode=(SELECT REPLACE(@@sql_mode, 'ONLY_FULL_GROUP_BY', ''));

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
        
**List all user**
 
        getent passwd {1000..60000}

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

Laravel new installation show error 500. Try following:

    chmod 777 -R storage

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
        OR
        php artisan make:model -m Users <-- auto create migration & model.

**Add column to existing table**  

        php artisan make:migration add_votes_to_users_table --table=users
        
**Rollback/refresh spesifik table**  

        php artisan migrate:refresh --path=/database/migrations/fileName.php
        
Inside the generated file:  

        public function up()  
        {  
                Schema::table('users', function (Blueprint $table) {  
                        $table->string('profile')->nullable();
			$table->softDeletes();
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
 
**SAMPLE DROP COLUMN CONTAIN FOREIGN KEY**
        
        /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('kad_ahlis', function (Blueprint $table) {
            $table->dropForeign('kad_ahlis_status_id_foreign'); 
            $table->dropForeign('kad_ahlis_last_updated_by_foreign'); 
            $table->dropColumn(['nota','bukti_bayaran','status_id','last_updated_by']);
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::disableForeignKeyConstraints();
        Schema::table('kad_ahlis', function (Blueprint $table) {
            $table->text('nota');
            $table->text('bukti_bayaran');
            $table->unsignedBigInteger('status_id');
            $table->unsignedBigInteger('last_updated_by');

            $table->foreign('status_id')
                    ->references('id')->on('ref_statuses')
                    ->onDelete('cascade');

            $table->foreign('last_updated_by')
                    ->references('id')->on('users')
                    ->onDelete('cascade');
        });
    }


**MAKE NULLABLE**

        /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('email')->nullable()->change();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function (Blueprint $table) {
            $table->string('email')->change();
        });
    }
    
**Clear all cache memory**

        php artisan cache:clear  
        php artisan config:clear
        php artisan config:cache  
        php artisan view:clear 
        
        php artisan optimize:clear
        
**Download Excel (csv)**

        $array_data = array();
        $myFile = public_path('location/folder')."/download".date("Y_m_d_H_i_s").".csv"; //save in folder.
        $fh = fopen($myFile, 'a') or die("can't open file");
        $stringData = "";
        foreach ($array_data as $data_column) {
            $stringData = $stringData.$data_column[0].",".$data_column[1].",".$data_column[2].",".$data_column[3]"\n"; 
        }
        $stringData = $stringData."\nTarikh muat turun: ".date("Y-m-d H:i:s");

        fwrite($fh, $stringData);
        fclose($fh);

        //download EXCEL (CSV)
        if (file_exists($myFile)) {
            header('Content-Description: File Transfer');
            header('Content-Type: application/octet-stream');
            header('Content-Disposition: attachment; filename='.basename($myFile));
            header('Content-Transfer-Encoding: binary');
            header('Expires: 0');
            header('Cache-Control: must-revalidate');
            header('Pragma: public');
            header('Content-Length: ' . filesize($myFile));
            ob_clean();
            flush();
            readfile($myFile);
            unlink($myFile); //delete file
            exit;
        }
        
**Ajax request**
        
        //controller
         public function find_cawangan_kecil(Request $request)
    {
        if ($request->has('cawangan_id')) {
            $cawangan_kecil = ref_cawangan_kecil::where('cawangan_id',$request->cawangan_id)->get();
            return $cawangan_kecil->pluck('penerangan','id');
        }
        
    }
        
        {{-- html component: change the cawangan list to populate the cawangan kecil  --}}
        <strong>Cawangan:</strong>
        {!! Form::select('cawangan_id', $ref_cawangan->pluck('nama','id'),null,['class' => 'form-control selectComponent dynamic'. ($errors->has('cawangan_id') ? ' is-invalid' : null) ,'placeholder' => 'Pilih jabatan...', 'id' => 'cawangan']); !!}
        
        
        <strong>Cawangan kecil:</strong>
        <select class="form-control selectComponent {{ ($errors->has('cawangan_kecil_id') ? ' is-invalid' : null) }}" name="cawangan_kecil_id" id="cawangan_kecil" placeholder="SILA PILIH CAWANGAN"><option>--Sila Pilih--</option></select>
        
        //script section
        $(".container").on('change','#cawangan',function() {
            if($(this).val() != ''){
                $.ajax({
                    type: "GET",
                    // dataType: "html",
                    url: "{{ route('request.find_cawangan_kecil') }}",
                    data: {'cawangan_id': $("#cawangan").val() },
                    beforeSend: function(){
                         // $("#loading").show();
                       },
                    success: function(response) 
                    {    
                        // $("#loading").hide();   
                        console.log(response);
                        $("#cawangan_kecil").empty();
                        $("#cawangan_kecil").append('<option>--Sila Pilih Cawangan Kecil--</option>');
                        if(response)
                        {
                            $.each(response,function(key,value){
                                $('#cawangan_kecil').append($("<option/>", {
                                   value: key,
                                   text: value
                                }));
                            });
                        }
                    }
                });
            }
        });

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
        
# OPENLITESPEED ADD NEW SUBDOMAIN

https://www.youtube.com/watch?v=Xy_GUip3qAo  

1. Goto LiteSpeed WebAdmin Console: `xxx.xxx.xx.xx:7080` . Get admin password: `cat .litespeed_password`
2. Add `Virtual Hosts`, add `Virtual Host Name`, add `Virtual Host Root` create folder, add `Config File` create by copy other config and change accordingly.

        Virtual Host Name: yourwebsite.com
        Virtual Host Root: /var/www/yourwebsite.com
        Config File: /usr/local/lsws/conf/vhosts/wordpress/yourwebsite.com.conf
        Enable Scripts/ExtApps: Yes
        Restrained: No
        External App Set UID Mode: DocRoot UID <-- must, otherwise rewrite .htaccess not working

**`Config File` syntax as below** 

        docRoot                   /var/www/yourwebdir.com

        index  {
          useServer               0
          indexFiles              index.php index.html
        }

        rewrite  {
          enable                  1
          autoLoadHtaccess        1
        }

        
3. Add `Listener`. Choose listener by their Port. If the listener is existed, do not add new listener.
4. Generate LetsEncrypt Cert:

        certbot certonly --webroot -w /var/www/html -d www.yourwebsite.com 
        
5. Add SSL path at `Virtual Hosts > Choose domain > SSL` add add the following:

        Private Key File: /etc/letsencrypt/live/www.yourwebsite.com/privkey.pem
        Certificate File: /etc/letsencrypt/live/www.yourwebsite.com/fullchain.pem
        
6. Do not add SSL path to the `Listener`

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
        
        1. Create new repo in github.
        2. git remote add origin <github repo url> <- local machine
        3. git push -u origin master OR git push -f origin master <- local machine
        
**Google SMTP setting**

        SMTP Host: smtp.gmail.com
        Encryption: TLS
        SMTP Port: 587
        SMTP Username: without @gmail
        
https://myaccount.google.com/lesssecureapps


# Windows Hack

**Copy large file via xcopy - copy large files**

        xcopy /E C:\xampp\htdocs C:\laragon\www
        
**Delete large forlder**

        RMDIR /Q/S foldername

**VESTACP SSL LetsEncrypt Renew Problem

        https://community.letsencrypt.org/t/solved-vestacp-and-cloudflare-error-let-s-encrypt-new-auth-status-429-rate-limit/119649
        
             
---



# ==========
# install lamp but skip php
# ==========
    https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04
    1. sudo apt update
    2. sudo apt -y upgrade
    3. sudo apt install apache2
    4. sudo apt install mysql-server

# install PHP 8
    https://www.cloudbooklet.com/how-to-upgrade-php-version-to-php-8-0-on-ubuntu/
    
    https://computingforgeeks.com/how-to-install-php-on-ubuntu-2/

    ==Ubuntu 20.04|18.04 (Not needed on Ubuntu 22.04)==
    5. sudo apt install lsb-release ca-certificates apt-transport-https software-properties-common -y
    6. sudo add-apt-repository ppa:ondrej/php
    7. sudo apt install php8.0
    8. sudo apt install composer

    9. install php module

        sudo apt install php8.0-amqp php8.0-common php8.0-gd php8.0-ldap php8.0-odbc php8.0-readline php8.0-sqlite3 php8.0-xsl php8.0-curl php8.0-gmp php8.0-mailparse php8.0-opcache php8.0-redis php8.0-sybase php8.0-yac php8.0-ast php8.0-dba php8.0-igbinary php8.0-mbstring php8.0-pgsql php8.0-rrd php8.0-tidy php8.0-yaml php8.0-bcmath php8.0-dev php8.0-imagick php8.0-memcached php8.0-phpdbg php8.0-smbclient php8.0-uuid php8.0-zip php8.0-bz2 php8.0-ds php8.0-imap php8.0-msgpack php8.0-pspell php8.0-snmp php8.0-xdebug php8.0-zmq php8.0-cgi php8.0-enchant php8.0-interbase php8.0-mysql php8.0-psr php8.0-soap php8.0-xhprof php8.0-cli php8.0-fpm php8.0-intl php8.0-oauth php8.0-raphf php8.0-solr php8.0-xml 

    ==choose PHP==
    https://readerstacks.com/how-to-install-php-8-0-in-ubuntu-20-04-lts/
    10. sudo update-alternatives --config php <-- choose php 8.0

# allow mysql
    /etc/mysql/mysql.conf.d/mysqld.cnf <-- bind-address = 0.0.0.0

    CREATE USER 'skp'@'%' IDENTIFIED BY 'skp@123';
    GRANT ALL ON *.* TO 'skp'@'%' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    sudo ufw allow 3306

# todo
    1. change root path to /var/www/html/public in /etc/apache2/sites-available/000-default.conf
    2. add .env and change the db credential
    3. sudo chown -R www-data:www-data storage
    4. sudo a2enmod rewrite
    5. add the following in /etc/apache2/sites-available/000-default.conf

    <Directory /var/www/html/public>
		Options Indexes FollowSymLinks
	 	AllowOverride All
	</Directory>

    sudo cp /etc/apache2/sites-available/example.com.conf /etc/apache2/sites-available/test.com.conf

    <VirtualHost *:80>
        ServerAdmin admin@example.com
        ServerName example.com
        ServerAlias www.example.com
        DocumentRoot /var/www/example.com/public_html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>

    sudo a2ensite dev.puspanita.org.my
    sudo a2dissite 000-default.conf

    sudo apt install certbot python3-certbot-apache

    sudo ufw enable
    sudo ufw app list
    sudo ufw allow in "Apache Full"

    sudo certbot --apache
    sudo systemctl status certbot.timer <-- check status
    sudo certbot renew --dry-run <-- test renewal process

# Install/update Composer

	sudo apt-get remove composer
	sudo apt-get update
	sudo apt-get install curl
	sudo curl -s https://getcomposer.org/installer | php
	sudo mv composer.phar /usr/local/bin/composer

# SSL certbot

not tested yet
	certbot --nginx -d example.com -d www.example.com

	==FOR REFERENCE PURPOSE ONLY==
	IMPORTANT NOTES:
    - Congratulations! Your certificate and chain have been saved at:
      /etc/letsencrypt/live/graduasi2.izoolz.com/fullchain.pem
      Your key file has been saved at:
      /etc/letsencrypt/live/graduasi2.izoolz.com/privkey.pem
      Your cert will expire on 2022-11-15. To obtain a new or tweaked
      version of this certificate in the future, simply run certbot again
      with the "certonly" option. To non-interactively renew *all* of
      your certificates, run "certbot renew"
    - If you like Certbot, please consider supporting our work by:

      Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
      Donating to EFF:                    https://eff.org/donate-le


ref
	https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04



You can learn about your existing certificates by checking out the old domain's .conf file in /etc/letsencrypt/renewal/ or by doing sudo certbot certificates

If you have changed domain name you can just install the nginx plugin

```sudo apt install python-certbot-nginx```

obtain a new certificate with

``sudo certbot -d [newdomain.tld] --nginx``

Afterwards you can check if there are any old, no longer needed certificates configured with

```sudo certbot certificates```

You will likely find an entry there for the cert with your old domain name. Remove that with

```sudo certbot delete```

and interactively pick which old ones to delete. This is important so that later you can just issue sudo certbot renew and not get errors due to the no longer relevant domain failing to authorize.

Restart nginx and you are done.



# NGIX add multiple website

ref - https://webdock.io/en/docs/how-guides/shared-hosting-multiple-websites/how-configure-nginx-to-serve-multiple-websites-single-vps

1. add new dir ```mkdir /var/www/html/yourwebfolder.com```
2. change permission ```chown -R www-data:www-data /var/www/html/yourwebfolder.com```
3. add or edit config ```nano /etc/nginx/sites-available/yourwebfolder.com.conf```

		server {
			listen 80;
			server_name yourwebfolder.com;
			root /var/www/yourwebfolder.com;

			add_header X-Frame-Options "SAMEORIGIN";
			add_header X-XSS-Protection "1; mode=block";
			add_header X-Content-Type-Options "nosniff";

			index index.php;

			charset utf-8;

			location / {
				try_files $uri $uri/ /index.php?$query_string;
			}

			location = /favicon.ico { access_log off; log_not_found off; }
			location = /robots.txt  { access_log off; log_not_found off; }

			error_page 404 /index.php;

			location ~ \.php$ {
				fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
				fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
				include fastcgi_params;
			}

			location ~ /\.(?!well-known).* {
				deny all;
			}
		}

4. copy link using ```ln -s /etc/nginx/sites-available/yourwebfolder.com.conf /etc/nginx/sites-enabled/```
5. nginx -t (check status & the following will show if OK)

		nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
		nginx: configuration file /etc/nginx/nginx.conf test is successful

6. restart ngix

		systemctl restart nginx

7. MAKE SURE POINT DNS A RECORD TO SERVER IP. do with www & without www
8. Add SSL

		certbot --nginx -d yourwebfolder.com

9. during wizard, choose to redirect to https. If forgot, add the following at the bottom of ```/etc/nginx/sites-available/yourwebfolder.com.conf```

		server {
			if ($host = graduasi2.izoolz.com) {
				return 301 https://$host$request_uri;
			} # managed by Certbot


			listen 80;
			server_name _ graduasi2.izoolz.com www.graduasi2.izoolz.com;
			return 404; # managed by Certbot


		}

10. UPGRADE PHP7 to 8.1
	1. sudo apt update && sudo apt -y upgrade
	2. sudo systemctl reboot
	3. sudo apt update
	4. sudo apt install lsb-release ca-certificates apt-transport-https software-properties-common -y
	5. sudo add-apt-repository ppa:ondrej/php
	6. sudo apt install --no-install-recommends php8.1
	7. sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath
	8. 
	9. sudo apt install php8.1-fpm
	10. change php7.4-fpm.sock to php8.1-fpm.sock in /etc/nginx/sites-available/xxxx.conf
	
		location ~ \.php$ {
			fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
			fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
			include fastcgi_params;
		    }

# FIREWALL

1. VestaCP firewall via CLI (make sure call full path sudo /usr/local/vesta/bin/)

	1. refer https://vestacp.com/docs/cli/ for syntax
	2. sample to list rule ``sudo /usr/local/vesta/bin/v-list-firewall``
	3. sample to delete rule ``sudo /usr/local/vesta/bin/v-delete-firewall-rule 12``
	4. sample to add rules ``sudo /usr/local/vesta/bin/v-add-firewall-rule ACCEPT 192.168.1.1/0 8383 TCP test``
