---
title: Nextcloud Ubuntu Install
layout: default
---

## Ubuntu Server Installation - Nextcloud

## Part 1: Setting Up Cloudflare

1. **Ensure Your Domain Uses Cloudflare DNS**
   ![2024-09-30_16-55](https://github.com/user-attachments/assets/25d76502-6cfe-4c63-8c04-1a287376c9f8)

2. **[Cloudflare Zero Trust](https://one.dash.cloudflare.com)**

3. **Navigate to Network > Tunnels**.

4. **Create the tunnel and select Debian 64 Bit architecture.**

5. **Copy the Cloudflared Connector and install on Ubuntu 24.04 server**

6. **Configure Public Hostname**  
 ![2024-09-30_17-18](https://github.com/user-attachments/assets/a1ce0a62-b72d-4d92-99b7-fb2f38f6cda7)


### Step 2: Downloading and Installing Nextcloud

1. **Download Nextcloud:**

    ```bash
    wget https://download.nextcloud.com/server/releases/latest.zip
    ```

2. **Install MariaDB:**

    ```bash
    sudo apt install -y mariadb-server
    sudo mysql_secure_installation
    ```
2a. **mysql_secure_installation:**

	- Enter current password for root (enter for none): ```Enter```
	- Switch to unix_socket_authentication [Y/n]  ```n```
	- Change root password [Y/n]  ```Y```
	- Set password
	- Remove anonymous users?  ```Y```
	- Disallow root login remotely?  ```Y```
	- Remove test database and access to it? ```Y```
	- Reload privilidge tables now?  ```Y```


3. **Create Nextcloud Database:**

    ```bash
    sudo mariadb
    ```

    Inside the MariaDB shell:

    ```sql
    CREATE DATABASE nextcloud;
    GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';
    FLUSH PRIVILEGES;
    ```

    Exit with `CTRL+D`.


### Step 4: Adjust PHP Settings

1. **Edit PHP Configuration:**

    ```bash
    sudo nano /etc/php/8.3/apache2/php.ini
    ```

    !!! note "php.ini"
        Some of these will need changed others will need uncommnented and changed.

		memory_limit = 512M
		
		upload_max_filesize = 2G
		
		max_execution_time = 360
		
		post_max_size = 2G
		
		date.timezone = America/New_York
		
		opcache.enable = 1
		
		opcache.interned_strings_buffer = 32
		
		opcache.max_accelerated_files = 10000
		
		opcache.memory_consumption = 128
		
		opcache.save_comments = 1
		
		opcache.revalidate_freq = 1
		
2. **Enable acpu module**

	```bash
	sudo nano /etc/php/8.3/mods-available/apcu.ini
	```
	
	Add to the file: `apc.enable_cli=1`

3. **Restart Apache:**

    ```bash
    sudo systemctl restart apache2
    ```


## Part 3: Dealing with Warnings

1. **Change config.php permissions**

	```bash
	sudo chmod 660 /var/www/my.justaguylinux.cloud/config/config.php
	```

2. **Detected some missing optional indices.**

	Make occ executable: 
	
	```bash
	sudo chmod +x /var/www/my.justaguylinux.cloud/occ
	```
	
	Then add missing indices:
	
	```bash
	sudo -u www-data /var/www/my.justaguylinux.cloud/occ db:add-missing-indices
	```
	
	Check to see if error is no longer
	
3. **One or more mimetype migrations are available**

	```bash
	sudo -u www-data /var/www/my.justaguylinux.cloud/occ maintenance:repair --include-expensive
	```
	
4.  **The following warnings**
	
	- Server has no maintenance window start time configured.

	- The database is used for transactional file locking.

	- No memory cache has been configured.

	- Your installation has no default phone region set.

	- You have not set or verified your email server configuration yet.
	
	```bash
	sudo nano /var/www/my.justaguylinux.cloud/config/config.php
	```
	**Remove:** `maintenance => false,`
	
	**Add:**
	
	```php
    'mail_from_address' => 'nextcloud',
	'mail_smtpmode' => 'smtp',
	'mail_sendmailmode' => 'smtp',
	'mail_domain' => 'gmail.com',
	'mail_smtphost' => 'smtp.gmail.com',
	'mail_smtpport' => '587',
	'mail_smtpauth' => 1,
	'mail_smtpname' => 'nextcloud@gmail.com',
	'mail_smtppassword' => 'app-specific-pw',
	'maintenance_window_start' => 1,
	'memcache.local' => '\\OC\\Memcache\\APCu',
	'memcache.distributed' => '\\OC\\Memcache\\Redis',
	'memcache.locking' => '\\OC\\Memcache\\Redis',
	'redis' => 
	array (
	'host' => 'localhost',
	'port' => 6379,
	),
	'default_phone_region' => 'US',
	'overwriteprotocol' => 'https',
	```

5. **Some headers are not set correctly on your instance - The `Strict-Transport-Security` HTTP header is not set (should be at least `15552000` seconds). **

	```bash
	sudo nano /etc/apache2/sites-available/my.justaguylinux.cloud-le-ssl.conf
	```
	
	**Add after 
	
```SSL configuration (done by Certbot)
    SSLCertificateFile /etc/letsencrypt/live/my.justaguylinux.cloud/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/my.justaguylinux.cloud/privkey.pem
```

Paste

```   
# HSTS header
    Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
```

```bash
sudo systemctl restart apache2
```

