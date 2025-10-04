# Downgrade PHP Version untuk Moodle

Moodle memerlukan PHP versi 8.0. Berikut cara downgrade dari PHP 8.3 ke PHP 8.0.

## Langkah-langkah Downgrade PHP

### 1. Tambahkan Repository Ondrej PHP

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```

### 2. Install PHP 8.0

```bash
sudo apt install php8.0 -y
```

### 3. Disable PHP 8.3 dan Enable PHP 8.0 pada Apache

```bash
sudo a2dismod php8.3
sudo a2enmod php8.0
sudo systemctl restart apache2
```

### 4. Set PHP 8.0 sebagai Default

```bash
sudo update-alternatives --set php /usr/bin/php8.0
sudo update-alternatives --set phar /usr/bin/phar8.0
sudo update-alternatives --set phar.phar /usr/bin/phar.phar8.0
```

### 5. Install Extension PHP 8.0 yang Diperlukan

```bash
sudo apt install php8.0-common php8.0-cli php8.0-fpm php8.0-mysql php8.0-curl php8.0-gd php8.0-mbstring php8.0-xml php8.0-zip php8.0-intl php8.0-soap php8.0-ldap php8.0-xmlrpc -y
```

### 6. Restart Apache

```bash
sudo systemctl restart apache2
```

## Verifikasi Instalasi

Cek versi PHP yang aktif:

```bash
php -v
```

Cek konfigurasi PHP di Apache:

```bash
sudo systemctl status apache2
```

## Troubleshooting

### Jika masih menggunakan PHP 8.3

```bash
sudo a2dismod php8.3
sudo a2enmod php8.0
sudo systemctl reload apache2
```

### Jika extension tidak terdeteksi

```bash
sudo apt install php8.0-dev -y
sudo systemctl restart apache2
```
