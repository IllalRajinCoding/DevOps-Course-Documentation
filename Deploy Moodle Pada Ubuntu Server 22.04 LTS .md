# Deploy Moodle Pada Ubuntu Server 22.04 LTS

## Setup Awal

- Instalasi Ubuntu Server 22.04 LTS gunakan ubuntu/jammy64
- Bisa menggunakan Vagrant dengan VirtualBox untuk virtualisasi

## Langkah-langkah Instalasi

```bash
sudo apt update && sudo apt upgrade
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

### 5. Troubleshooting: Jika Terjadi Error untuk Git Clone

```bash
sudo chattr -i /etc/resolv.conf
sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
```
