# LaravelBoilerplate_Ec2-ubuntu-
# Laravel Boilerplate

A boilerplate for deploying Laravel applications on an AWS EC2 instance. This project provides a template to quickly set up and deploy a Laravel application with necessary configurations and tools.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This repository contains a Laravel application boilerplate configured for deployment on an AWS EC2 instance. It includes configurations for Apache, environment settings, and necessary npm and composer dependencies.

## Prerequisites

Before you begin, ensure you have met the following requirements:

- AWS EC2 instance running Ubuntu.
- Basic knowledge of Linux command line.
- Apache, PHP, Composer, and Node.js installed on your server.

## Setup Instructions

Follow these steps to set up the Laravel application on your EC2 instance:

1. **Update and Upgrade Your System**

    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

2. **Install Necessary Packages**

    ```bash
    sudo apt install apache2 php libapache2-mod-php php-mysql unzip curl git -y
    sudo apt install php-cli php-mbstring php-xml php-bcmath php-json php-curl -y
    ```

3. **Install Composer**

    ```bash
    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/local/bin/composer
    ```

4. **Clone the Laravel Project**

    ```bash
    cd /var/www
    sudo git clone https://github.com/your-repo/laravel-boilerplate.git
    cd laravel-boilerplate
    ```

5. **Install Project Dependencies**

    ```bash
    sudo npm install
    sudo composer install
    ```

6. **Set Up Environment File**

    ```bash
    sudo cp .env.example .env
    ```

7. **Generate Application Key**

    ```bash
    sudo php artisan key:generate
    ```

8. **Set Proper Permissions**

    ```bash
    sudo chown -R www-data:www-data /var/www/laravel-boilerplate
    sudo chmod -R 775 /var/www/laravel-boilerplate/storage /var/www/laravel-boilerplate/bootstrap/cache
    ```

9. **Configure Apache**

    - Create Virtual Host Configuration

      ```bash
      sudo nano /etc/apache2/sites-available/laravel-boilerplate.conf
      ```

      Add the following content:

      ```apache
      <VirtualHost *:80>
          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/laravel-boilerplate/public
          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined

          <Directory /var/www/laravel-boilerplate/public>
              AllowOverride All
          </Directory>
      </VirtualHost>
      ```

    - Enable the New Site and Rewrite Module

      ```bash
      sudo a2dissite 000-default.conf
      sudo a2ensite laravel-boilerplate.conf
      sudo a2enmod rewrite
      sudo systemctl reload apache2
      ```

10. **Create and Enable Swap Space (if necessary)**

    ```bash
    sudo fallocate -l 1G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    sudo swapon --show
    ```

## Usage

After setting up, you can manage your Laravel application using the following commands:

- **Start Development Server**

    ```bash
    sudo npm run dev
    ```

- **Clear and Cache Configurations**

    ```bash
    sudo php artisan config:cache
    sudo php artisan cache:clear
    ```

## Troubleshooting

**1. Webpack Compilation Errors**

- **Issue:** Errors during `npm run dev`.

- **Solution:** Ensure `laravel-mix` is listed only once in `package.json` and re-run `npm install`.

**2. Apache Configuration Issues**

- **Issue:** Default Apache pages shown instead of Laravel.

- **Solution:** Check Apache virtual host configuration and ensure `DocumentRoot` and directory permissions are set correctly.

**3. File Permission Issues**

- **Issue:** Laravel logs and caching not functioning.

- **Solution:** Adjust file permissions for `storage` and `bootstrap/cache` directories.

**4. PHP Artisan Commands**

- **Issue:** Need to clear and cache configuration.

- **Solution:** Run `php artisan config:cache` and `php artisan cache:clear`.

**5. Adding Swap Space**

- **Issue:** Out of memory errors during `npm install`.

- **Solution:** Add swap space to prevent out-of-memory errors.

![Screenshot 2024-07-25 125955](https://github.com/user-attachments/assets/42666c8e-3b78-48ed-a785-0ac880a8c314)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to adjust the content as needed to fit your project’s specifics or personal preferences.
