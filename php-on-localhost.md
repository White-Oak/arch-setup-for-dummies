Installs LNMP (Linux + Nginx + MySQL + PHP) to an Arch installation.

1. `yaourt -S mariadb php php-fpm nginx phpmyadmin php-mcrypt --noconfirm`
2. `sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql`
3. `sudo systemctl enable mysqld.service`
4. `sudo systemctl start mysqld.service`
5. `mysql -u root -p`
  1. When prompted about password, press Enter.
  2. Query following (Replace `%name%` and `%pass%` with your preferences):

    ```
    CREATE USER '%name%'@'localhost' IDENTIFIED BY '%pass%';
    GRANT ALL PRIVILEGES ON *.* TO '%name%'@'localhost' WITH GRANT OPTION;
    ```
5. Instead of existing `server` blocks in `/etc/nginx/nginx.conf` enter following:
  ```
  server {
        listen       80;
        server_name  localhost;
        
        root   /usr/share/nginx/html;

        index  index.php index.html index.htm;

        location ~ \.php$ {
            fastcgi_pass   unix:/run/php-fpm/php-fpm.sock;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }
    }

    server {
         server_name     phpmyadmin.localhost;
 
         root    /usr/share/webapps/phpMyAdmin;
         index   index.php;
 
         location ~ \.php$ {
                 try_files      $uri =404;
                 fastcgi_pass   unix:/run/php-fpm/php-fpm.sock;
                 fastcgi_index  index.php;
                 include        fastcgi.conf;
         }
    }
  ```
6. Edit `/etc/php/php.ini`:
    1. `open_basedir` line should look somewhat like that: `open_basedir = /usr/share/webapps/:/srv/http/:/usr/share/nginx/html/:/home/:/tmp/:/usr/share/pear/:/etc/webapps/`
    2. Uncomment next lines `extension=mcrypt.so`,`extension=mysql.so`, `extension=mysqli.so` to enable needed extensions
7. `sudo chmod 777 /usr/share/nginx/html` -- _Never do this on production!_
8. `sudo systemctl start nginx.service php-fpm.service`
9. `sudo systemctl enable nginx.service php-fpm.service`
10. Put your php files in `/usr/share/nginx/html`.
11. Go to `localhost` and `phpmyadmin.localhost`. Default mysql user and password are `root` and ``, respectively.
