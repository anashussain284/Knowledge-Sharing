# Setup Larvel development with PostgreSQL

## Check postgreSQL version in linux

You can check postgreSQL version in linux use below commands

```
postgres --version
```
or
```
postgres -V
```
If you can't find the version on linux pc, To install PostgreSQL on your Linux system, follow these steps:

### 1. **Update Your Package List**
Open a terminal and update your package list to ensure that you have the latest information on the available packages.

```bash
sudo apt update
```

### 2. **Install PostgreSQL**
Install PostgreSQL along with its `contrib` package (which adds additional utilities and functionality).

```bash
sudo apt install postgresql postgresql-contrib
```

### 3. **Start PostgreSQL Service**
After the installation, PostgreSQL should automatically start. You can confirm by checking the service status:

```bash
sudo systemctl status postgresql
```

If it's not running, you can start it with:

```bash
sudo systemctl start postgresql
```

### 4. **Enable PostgreSQL to Start on Boot**
To make sure PostgreSQL starts automatically on system boot:

```bash
sudo systemctl enable postgresql
```

### 5. **Switch to the Default PostgreSQL User**
PostgreSQL creates a default user called `postgres`. You can switch to this user to interact with the PostgreSQL database:

```bash
sudo -i -u postgres
```

### 6. **Access the PostgreSQL Shell**
Once you are logged in as the `postgres` user, you can access the PostgreSQL interactive terminal (psql) by typing:

```bash
psql
```

### 7. **Create a New Database User (Optional)**
If you want to create a new user:

```sql
CREATE USER your_username WITH PASSWORD 'your_password';
```


### 8. **Create a New Database (Optional)**
You can also create a new database and grant all privileges to the new user:

```sql
CREATE DATABASE your_database;
GRANT ALL PRIVILEGES ON DATABASE your_database TO your_username;
```

### 9. **Exit PostgreSQL Shell**
To exit the PostgreSQL shell, type:

```sql
\q
```

Now you have PostgreSQL installed and ready to use! Let me know if you need help with configuring or using it further.To install PostgreSQL on your Linux system, follow these steps:

### 1. **Update Your Package List**
Open a terminal and update your package list to ensure that you have the latest information on the available packages.

```bash
sudo apt update
```

### 2. **Install PostgreSQL**
Install PostgreSQL along with its `contrib` package (which adds additional utilities and functionality).

```bash
sudo apt install postgresql postgresql-contrib
```

### 3. **Start PostgreSQL Service**
After the installation, PostgreSQL should automatically start. You can confirm by checking the service status:

```bash
sudo systemctl status postgresql
```

If it's not running, you can start it with:

```bash
sudo systemctl start postgresql
```

### 4. **Enable PostgreSQL to Start on Boot**
To make sure PostgreSQL starts automatically on system boot:

```bash
sudo systemctl enable postgresql
```

### 5. **Switch to the Default PostgreSQL User**
PostgreSQL creates a default user called `postgres`. You can switch to this user to interact with the PostgreSQL database:

```bash
sudo -i -u postgres
```

### 6. **Access the PostgreSQL Shell**
Once you are logged in as the `postgres` user, you can access the PostgreSQL interactive terminal (psql) by typing:

```bash
psql
```

### 7. **Create a New Database User (Optional)**
If you want to create a new user:

```sql
CREATE USER your_username WITH PASSWORD 'your_password';
```

### 8. **Create a New Database (Optional)**
You can also create a new database and grant all privileges to the new user:

```sql
CREATE DATABASE your_database;
GRANT ALL PRIVILEGES ON DATABASE your_database TO your_username;
```

### 9. **Exit PostgreSQL Shell**
To exit the PostgreSQL shell, type:

```sql
\q
```

Now you have PostgreSQL installed and ready to use! Let me know if you need help with configuring or using it further.

---

# Setup PostgreSQL on Laravel project

To set up PostgreSQL for your new Laravel project, follow these steps:

### 1. **Install PostgreSQL (if not installed)**
If you haven't installed PostgreSQL yet, follow the steps in my previous message to install it on your Linux system.

### 2. **Install Laravel Project**
If you haven't created your Laravel project yet, install it with the following command:

```bash
composer create-project --prefer-dist laravel/laravel your_project_name
```

Navigate to your project directory:

```bash
cd your_project_name
```

### 3. **Install PostgreSQL PHP Extension**
Laravel requires the PHP extension for PostgreSQL to work with it. Install it by running:

```bash
sudo apt install php-pgsql
```

After installation, restart your web server (Apache/Nginx) or PHP service:

```bash
sudo systemctl restart apache2  # For Apache
# or
sudo systemctl restart nginx    # For Nginx
```

### 4. **Create a PostgreSQL Database**
Log in to PostgreSQL as the default `postgres` user:

```bash
sudo -i -u postgres
psql
```

Create a new database and user:

```sql
CREATE DATABASE your_database_name;
CREATE USER your_username WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;
```

Exit the PostgreSQL shell:

```sql
\q
```

### 5. **Configure Laravel to Use PostgreSQL**
In your Laravel project, update the `.env` file to use PostgreSQL. Replace the following database environment variables:

```ini
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=your_database_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 6. **Update `config/database.php`**
By default, Laravel already includes PostgreSQL in its database configuration file. You can find the configuration in `config/database.php` under the `pgsql` section. It should look something like this:

```php
'pgsql' => [
    'driver' => 'pgsql',
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '5432'),
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
    'charset' => 'utf8',
    'prefix' => '',
    'schema' => 'public',
    'sslmode' => 'prefer',
],
```

### 7. **Run Migrations**
After configuring the database, run the Laravel migrations to create the necessary tables in PostgreSQL:

```bash
php artisan migrate
```

This should successfully create your Laravel application's database structure using PostgreSQL.

### 8. **Test the Connection**
To ensure everything is working correctly, you can try creating a new database model, running migrations, or interacting with the database using Eloquent. For example:

```bash
php artisan tinker
```

Then, try querying your database:

```php
DB::connection()->getPdo();
```

If you get a successful connection, you're ready to go!

Let me know if you need help with any specific step or configuration.

---

# Php extension installation for postgresql

The error indicates that the PostgreSQL module (`pgsql`) is not installed or enabled in your PHP installation. To resolve this, you need to install and enable the `pgsql` module for PHP.

Here's how you can fix this issue on your Linux system:

### 1. **Install the PostgreSQL PHP Extension**

First, install the `pgsql` extension for PHP. Depending on the PHP version you're using, the command might differ slightly.

For PHP 8.2:
```bash
sudo apt install php8.2-pgsql
```

For PHP 7.2 (if you're using multiple versions like you mentioned earlier):
```bash
sudo apt install php7.2-pgsql
```

### 2. **Enable the `pgsql` Extension**

After installing the `pgsql` extension, you need to enable it.

For PHP 8.2:
```bash
sudo phpenmod pgsql
```

For PHP 7.2:
```bash
sudo phpenmod -v 7.2 pgsql
```

### 3. **Restart the Web Server**

After enabling the extension, restart your web server to apply the changes:

For Apache:
```bash
sudo systemctl restart apache2
```

For Nginx (if you're using PHP-FPM):
```bash
sudo systemctl restart php8.2-fpm
# or for PHP 7.2
sudo systemctl restart php7.2-fpm
```

### 4. **Verify the Extension is Loaded**

You can verify that the `pgsql` module is successfully installed and loaded by creating a `phpinfo.php` file in your web directory. 

Create the file:
```bash
sudo nano /var/www/html/phpinfo.php
```

Add the following content:
```php
<?php
phpinfo();
?>
```

Visit `http://localhost/phpinfo.php` in your browser, and search for "pgsql" to confirm that the extension is enabled.

---

## How to install PostgreSQL and PGAdmin on Ubuntu 22.04

[Youtube tutorial](https://www.youtube.com/watch?v=UGfteFq_6Co&ab_channel=Abstractprogrammer)

## How to Create Table in PostgreSQL | Create Table, set Primary key & Auto-Increment in PostgreSQL DB

[Youtube tutorial](https://www.youtube.com/watch?v=NhfeCY4FwGk&ab_channel=FundaOfWebIT)

