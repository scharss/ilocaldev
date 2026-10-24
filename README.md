# 🐳 iLocalDev - PHP Development Environment with Docker

A complete, optimized development environment for PHP applications with MySQL, ready to use with Docker. Perfect for developing applications that need QR codes, Excel handling, databases, and more.

## 🎯 What is this project for?

This project provides a **plug-and-play** development environment that includes:

- **🔧 Configurable PHP version** (8.3.36 by default) with Apache optimized for development — set `PHP_VERSION` in `.env` to use another patched PHP release.
- **🗄️ MySQL 8.0** with data persistence
- **⚙️ phpMyAdmin** for visual database management
- **📊 Pre-installed extensions** for QR codes, Excel, images, and more
- **🚀 Optimized configuration** for modern PHP applications

### 💼 Ideal use cases

This environment is intended for **Web Development** in general, not just a single application. You can have multiple projects living together in `src/`.

**Web Development**
- ✅ **Web Development** for corporate sites, blogs, landing pages, portfolios, intranets, and custom applications

**CMS**
- ✅ WordPress: themes, plugins, and multisite

**Applications and utilities**
- ✅ QR/NFC, attendance, inventory, and traceability systems
- ✅ School, business, customer, appointment, and reporting management
- ✅ Excel/CSV import/export and report generation
- ✅ Authentication, sessions, roles, and admin dashboards
- ✅ Image processing, QR code generation, and ZIP files
- ✅ Legacy PHP project migration and local testing
- ✅ Multi-project lab: multiple websites and databases in the same environment

> **Compatibility:** ilocaldev is intended for PHP applications served from the project root. Frameworks or CMSs that require `DocumentRoot` to point to `public/` need additional Apache configuration.

## 🛠️ Included Technologies

| Service | Version | Port |
|----------|---------|--------|
| **PHP** | 8.3.36 with Apache | 8080 |
| **MySQL** | 8.0 | 33061 |
| **phpMyAdmin** | 5.2.3 with PHP 8.3.36 | 8081 |

### 📦 Pre-installed PHP extensions

- `mysqli`, `pdo_mysql` - Database connections
- `gd` - Image and QR code processing
- `zip` - Compressed files and Excel
- `mbstring` - Multibyte string support
- `intl` - Internationalization (dates, numbers)
- `exif` - Image metadata
- `opcache` - Performance cache
- `bcmath`, `soap`, `xml` - Math and web services

### 🧰 Included tools

- Composer 2.x
- Git
- curl
- zip/unzip
- Apache with `mod_rewrite`
- OPcache

## 🚀 Quick Start

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed

### 1. Clone the repository:
```bash
git clone https://github.com/scharss/ilocaldev.git
cd ilocaldev
```

### 2. Start the environment:
```bash
docker compose up -d --build
```

### 3. Done! Access:
- 🌐 **Your application**: http://localhost:8080
- 💾 **phpMyAdmin**: http://localhost:8081
- 🗄️ **MySQL**: localhost:33061

## 🐘 Configurable PHP Version

The PHP version for the web container is defined in `.env`:

```env
PHP_VERSION=8.3.36
```

After changing it, rebuild and recreate the web container:

```bash
docker compose build --pull web
docker compose up -d --no-deps --force-recreate web
```

The minimum accepted versions are those that include the patches for CVE-2026-109618 and CVE-2026-109619: **8.2.35, 8.3.36, 8.4.27, or 8.5.12**.

## 🏗️ Project Structure

```
ilocaldev/
├── .env                  # Environment variables (PHP version and database credentials)
├── docker-compose.yml    # Docker services configuration
├── php/
│   ├── Dockerfile        # Custom PHP image
│   └── php.ini           # Optimized PHP configuration
├── phpmyadmin/
│   └── Dockerfile        # phpMyAdmin image with PHP 8.3.36
├── src/                  # Your PHP projects
│   └── ...
└── README.md
```

## 🔧 Database Configuration

### Credentials

MySQL credentials are defined in `.env`:

```env
MYSQL_ROOT_PASSWORD=...
MYSQL_DATABASE=...
MYSQL_USER=...
MYSQL_PASSWORD=...
```

> `docker compose config` resolves variables and prints the passwords. Do not share its output. To validate without printing anything, use `docker compose config --quiet`.
>
> `.env` contains local secrets. Do not commit or share it.

Host: `db` (inside Docker)  
Host: `localhost:33061` (from your PC)

### Connecting from PHP
```php
<?php
// PDO connection example
try {
    $pdo = new PDO('mysql:host=db;dbname=mi_base_de_datos', 'mi_usuario_db', 'YOUR_PASSWORD');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "✅ Successful MySQL connection";
} catch(PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}
?>
```

## 📝 Development

### 1. Add your code
Place all your PHP files in the `src/` folder:
```
src/
├── index.php
├── config/
├── includes/
└── your-application/
```

### 2. Install dependencies with Composer
```bash
# Enter the container
docker compose exec web bash

# Inside the container
composer install
composer require phpoffice/phpspreadsheet  # For Excel
composer require chillerlan/php-qrcode     # For QR codes
```

### 3. View logs in real time
```bash
docker compose logs -f web    # PHP/Apache logs
docker compose logs -f db     # MySQL logs
```

## 🎛️ Useful Commands

### Container management
```bash
# View container status
docker compose ps

# Stop everything
docker compose down

# Rebuild after changes
docker compose up -d --build

# Clean everything (DELETES DATA!)
docker compose down -v
```

### Container access
```bash
# Enter the web container (PHP)
docker compose exec web bash

# Enter the MySQL container
docker compose exec db mysql -u root -p
```

### Database backup
```bash
# Export database
docker compose exec db mysqldump -u root -p mi_base_de_datos > backup.sql

# Import database
docker compose exec -T db mysql -u root -p mi_base_de_datos < backup.sql
```

## ⚡ Optimized Configuration

### Increased limits for development
- **Memory limit**: 512MB
- **Upload max**: 64MB
- **Post max**: 64MB
- **Max file uploads**: 10
- **Max input nesting level**: 32
- **Execution time**: 120 seconds
- **Timezone**: America/Bogota

### To change settings
Edit `php/php.ini` and recreate the web container:
```bash
docker compose up -d --no-deps --force-recreate web
```

## 🌍 For Production

For production use, change the following in `php/php.ini`:
```ini
error_reporting = E_ALL & ~E_DEPRECATED
display_errors = Off
display_startup_errors = Off
opcache.validate_timestamps = 0
```

## 🤝 Contributing

1. Fork the project
2. Create your branch: `git checkout -b feature/new-feature`
3. Commit: `git commit -m 'Add new feature'`
4. Push: `git push origin feature/new-feature`
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License. See `LICENSE` for details.

## 🆘 Support

Having trouble? Open an [issue](https://github.com/scharss/ilocaldev/issues)!

---

⭐ **If you like this project, give it a star!** ⭐
