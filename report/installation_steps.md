```markdown
# Installation Steps

## 1. VM Setup

1. Launch VMware Workstation
2. Create two new virtual machines: www-vm and db-vm
   - Select Ubuntu Server 24.04.1 ISO
   - Allocate appropriate storage and memory
3. Initial setup:
   - Check for updates and upgrades:
     ```
     sudo apt-get update
     sudo apt-get upgrade
     ```
   - Identify the IP address of both virtual machines:
     ```
     ip address
     ```
     - www-vm IP address: 192.168.191.138
     - db-vm IP address: 192.168.191.139

## 2. Installing Apache, PHP, and phpMyAdmin on www-vm

1. Install Apache, PHP, and phpMyAdmin:
   ```
   sudo apt install apache2 php libapache2-mod-php php-mysql phpmyadmin
   ```

2. Link phpMyAdmin configuration to Apache:
   ```
   sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
   sudo a2enconf phpmyadmin
   sudo systemctl restart apache2
   ```

3. Test Apache by accessing the IP address of www-vm in a web browser.

## 3. Installing MySQL on db-vm

1. Install MySQL server:
   ```
   sudo apt install mysql-server
   ```

2. Secure the MySQL installation:
   ```
   sudo mysql_secure_installation
   ```

3. Modify the MySQL configuration:
   ```
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```
   Set the `bind-address` to db-vm's IP address.

4. Create WordPress database and user:
   ```sql
   CREATE DATABASE wordpress_db;
   GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wordpress_user'@'<www-vm-ip-address>';
   ```

5. Verify www-vm can access the MySQL database:
   ```
   mysql -u wordpress_user -h <db-vm-ip-address> -p
   ```

## 4. Configuring phpMyAdmin

1. Edit phpMyAdmin configuration on www-vm:
   ```
   sudo nano /etc/phpmyadmin/config.inc.php
   ```

2. Update the following:
   ```php
   $cfg['Servers'][$i]['host'] = '<db-vm-ip-address>';
   $cfg['Servers'][$i]['user'] = 'wordpress_user';
   $cfg['Servers'][$i]['password'] = '<mysql-password>';
   ```

3. Verify phpMyAdmin access through www-vm's IP address.

## 5. Installing WordPress

1. Download and extract WordPress:
   ```
   cd /tmp
   wget https://wordpress.org/latest.tar.gz
   tar -xzvf latest.tar.gz
   sudo mv wordpress/* /var/www/html/
   ```

2. Configure WordPress:
   ```
   cd /var/www/html
   sudo cp wp-config-sample.php wp-config.php
   sudo nano wp-config.php
   ```

3. Set proper permissions:
   ```
   sudo chown -R www-data:www-data /var/www/html
   sudo chmod -R 755 /var/www/html
   ```

## 6. Creating phpinfo page

1. Create a PHP test file:
   ```
   echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/phpinfo.php
   ```

2. Verify PHP is working by visiting: `http://<www-vm-ip>/phpinfo.php`

## 7. Firewall Configuration

On www-vm:
```
sudo ufw enable
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow from <db-vm-ip-address> to any port 3306
sudo ufw allow from <host-ip-address> to any port 22
```

On db-vm:
```
sudo ufw enable
sudo ufw allow from <www-vm-ip-address> to any port 3306
sudo ufw allow from <host-ip-address> to any port 22
```

## 8. Web Administrator Setup

1. Create web admin account:
   ```
   sudo adduser webadmin
   sudo usermod -aG www-data webadmin
   ```

2. Set permissions for /var/www/html:
   ```
   sudo chown -R www-data:www-data /var/www/html
   sudo chmod -R 775 /var/www/html
   ```

3. Edit SSH configuration:
   ```
   sudo nano /etc/ssh/sshd_config
   ```
   Add:
   ```
   AllowUsers webadmin
   PasswordAuthentication yes
   PermitRootLogin no
   ```

4. Restart SSH service:
   ```
   sudo systemctl restart ssh
   ```

5. Test SSH access:
   ```
   ssh webadmin@<www-vm-ip-address>
   ```

This setup allows the webadmin user to modify files in /var/www/html without having root access or the ability to modify system configurations.
```
