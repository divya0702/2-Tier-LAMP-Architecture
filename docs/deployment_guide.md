# Web Administrator Deployment Guide

This guide provides instructions for web administrators to manage and deploy content on the 2-tier LAMP platform.

## Accessing the Web Server

SSH into the web server VM:

```bash
ssh webadmin@<www-vm-ip-address>
```

## Managing WordPress

Access the WordPress admin panel at:

```
http://<www-vm-ip-address>/wp-admin
```

Use the credentials set during WordPress installation.

## Deploying Static HTML Files

1. Navigate to the web root directory:

```bash
cd /var/www/html
```

2. Create or upload your HTML file:

```bash
nano index.html
```

3. Set appropriate permissions:

```bash
sudo chown www-data:www-data index.html
sudo chmod 644 index.html
```

## Deploying PHP Scripts

1. Create a PHP file in the web root:

```bash
nano example.php
```

2. Set permissions:

```bash
sudo chown www-data:www-data example.php
sudo chmod 644 example.php
```

## Accessing phpMyAdmin

1. Open a web browser and navigate to:

```
http://<www-vm-ip-address>/phpmyadmin
```

2. Log in using the MySQL credentials set during installation.

## File System Permissions

- The web administrator has permissions to write in the Apache document directory (/var/www/html).
- You can install small PHP-based applications that do not require changes to Apache or database configurations.
- To modify files, use the `webadmin` user account, which is part of the `www-data` group.

## Security Considerations

- SSH access is restricted to the `webadmin` user.
- Root login via SSH is disabled.
- Always use HTTPS for secure communication when possible.

## Troubleshooting

If you encounter issues with file permissions:

1. Ensure the file is owned by www-data:
   ```bash
   sudo chown www-data:www-data /path/to/file
   ```

2. Set the correct permissions:
   ```bash
   sudo chmod 644 /path/to/file  # for regular files
   sudo chmod 755 /path/to/directory  # for directories
   ```
