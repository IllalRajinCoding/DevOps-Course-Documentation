# Deploy Moodle Pada Ubuntu Server 22.04 LTS

## Setup Awal

- Instalasi Ubuntu Server 22.04 LTS gunakan ubuntu/jammy64
- Bisa menggunakan Vagrant dengan VirtualBox untuk virtualisasi

## Langkah-langkah Instalasi

### Update Sistem

```bash
sudo apt update && sudo apt upgrade -y
```

### 1. Install Apache2

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl enable apache2.service
sudo systemctl start apache2.service
```

### 2. Install MariaDB

```bash
sudo apt install mariadb-server mariadb-client -y
sudo systemctl enable mariadb.service
sudo systemctl start mariadb.service
```

### 3. Install PHP dan Extension

```bash
sudo apt install php graphviz aspell ghostscript clamav php-pspell php-curl php-gd php-intl php-mysql php-xml php-xmlrpc php-ldap php-zip php-soap php-mbstring -y
```

### 4. Install Git dan Download Moodle

```bash
sudo apt install git -y
sudo git clone git://git.moodle.org/moodle.git
cd moodle
sudo git branch -a
sudo git checkout MOODLE_403_STABLE
```

### 5. Pindahkan File Moodle

```bash
sudo cp -R moodle /var/www/html/
sudo mkdir /var/www/moodledata
sudo chown -R www-data:www-data /var/www/html/moodle
sudo chown -R www-data:www-data /var/www/moodledata
sudo chmod -R 755 /var/www/html/moodle
sudo chmod -R 777 /var/www/moodledata
```

### 6. Konfigurasi Database

```bash
sudo mysql_secure_installation
sudo mysql -u root -p
```

Jalankan SQL berikut:

```sql
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'moodle'@'localhost' IDENTIFIED BY 'Password123#!';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodle'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 7. Konfigurasi config.php

Buat file config.php di direktori `/var/www/html/moodle`:

```bash
sudo nano /var/www/html/moodle/config.php
```

Salin konfigurasi berikut:

```php
<?php  // Moodle configuration file

unset($CFG);
global $CFG;
$CFG = new stdClass();

$CFG->dbtype    = 'mariadb';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'moodle';
$CFG->dbuser    = 'moodle';
$CFG->dbpass    = 'Password123#!';
$CFG->prefix    = 'mdl_';
$CFG->dboptions = array (
  'dbpersist' => 0,
  'dbport' => 3306,
  'dbsocket' => '',
  'dbcollation' => 'utf8mb4_general_ci',
);

$CFG->wwwroot   = 'http://localhost/moodle';
$CFG->dataroot  = '/var/www/moodledata';
$CFG->admin     = 'admin';

$CFG->directorypermissions = 0777;

require_once(__DIR__ . '/lib/setup.php');

// There is no php closing tag in this file,
// it is intentional because it prevents trailing whitespace problems!
```

### 8. Restart Service

```bash
sudo systemctl restart apache2
sudo systemctl restart mariadb
```

## Troubleshooting

### Jika Terjadi Error Git Clone (DNS Resolution)

```bash
sudo chattr -i /etc/resolv.conf
sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
```

### Error: Moodle requires the mbstring PHP extension

```bash
sudo apt install php-mbstring -y
sudo systemctl restart apache2
```

### Error: Moodle requires the xml PHP extension

```bash
sudo apt install php-xml -y
sudo systemctl restart apache2
```

### Error: Permission Denied

```bash
sudo chown -R www-data:www-data /var/www/html/moodle
sudo chown -R www-data:www-data /var/www/moodledata
sudo chmod -R 755 /var/www/html/moodle
sudo chmod -R 777 /var/www/moodledata
```

## Akses Moodle

Setelah semua konfigurasi selesai, akses Moodle melalui browser:

```
http://localhost/moodle
```

atau

```
http://[IP_ADDRESS]/moodle
```

Jika butuh downgrade [Downgrade PHP Version untuk Moodle](./Setting-Your-Version-On-PHP.md)
