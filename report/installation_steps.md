# Installation Steps

## 1. VM Setup

1. Launch VMware Workstation
2. Create two new virtual machines: www-vm and db-vm
   - Select Ubuntu Server 24.04.1 ISO
   - Allocate appropriate storage and memory

## 2. Installing Apache, PHP, and phpMyAdmin on www-vm

```bash
sudo apt update
sudo apt upgrade
sudo apt install apache2 php libapache2-mod-php php-mysql phpmyadmin
