# Laravel Project Setup with Apache Virtual Host

This guide provides step-by-step instructions to set up a Laravel project on an Apache server, enabling you to access it via a custom local domain without using `php artisan serve`.

## Prerequisites

- Apache Web Server
- Laravel Installed
- Sudo Privileges on the System

## Project Structure

Assuming your Laravel project is located at:
```plaintext
/var/www/html/l/frameworks/Laravel/abcd
```

## Step 1: Apache Virtual Host Configuration

1. **Create a Virtual Host Configuration File**:
   - Open Apache's configuration directory and create a new configuration file for your project.
   
   ```bash
   sudo nano /etc/apache2/sites-available/abcd.local.conf
   ```

2. **Add Virtual Host Configuration**:
   - Insert the following configuration, specifying your project directory and preferred local domain.

   ```apache
    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerAlias www.abcd.local
        ServerName abcd.local
        DocumentRoot "/var/www/html/l/frameworks/Laravel/abcd/public"

        <Directory "/var/www/html/l/frameworks/Laravel/abcd/public">
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/abcd-error.log
        CustomLog ${APACHE_LOG_DIR}/abcd-access.log combined
    </VirtualHost>
   ```

3. **Save and Close the File**.

## Step 2: Update the `/etc/hosts` File

1. Open the hosts file to map `abcd.local` to `localhost`:
   ```bash
   sudo nano /etc/hosts
   ```

2. Add this line at the end of the file:
   ```plaintext
   127.0.0.1 abcd.local
   ```

3. **Save and Close the File**.

## Step 3: Enable the Virtual Host and Restart Apache

1. **Enable the Site Configuration**:
   ```bash
   sudo a2ensite abcd.local.conf
   ```

2. **Reload Apache**:
   ```bash
   sudo systemctl restart apache2
   ```

## Step 4: Set Correct Permissions

Ensure that the web server has the necessary permissions to write to the `storage` and `bootstrap/cache` directories.

1. **Set Permissions**:
   ```bash
   cd /var/www/html/l/frameworks/Laravel/abcd
   sudo chmod -R 775 storage
   sudo chmod -R 775 bootstrap/cache
   ```

2. **Set Ownership**:
   - Replace `www-data` with your web server’s user if it differs (e.g., `apache` on some systems).
   
   ```bash
   sudo chown -R www-data:www-data storage
   sudo chown -R www-data:www-data bootstrap/cache
   ```

## Accessing Your Laravel Project

After completing the steps above, open your browser and navigate to:
```plaintext
http://abcd.local
```

You should see your Laravel project without needing `php artisan serve`!

## Troubleshooting

### Permission Issues
If you encounter errors related to permissions on `storage` or `bootstrap/cache`, re-run the permission and ownership commands in Step 4.

---

## Contact Information

For questions or further assistance, feel free to reach out.

- **Email**: anashussain284@gmail.com

---
