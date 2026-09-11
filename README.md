# 🐳 ilocaldev - PHP Development Environment with Docker

A complete and optimized development environment for PHP applications with MySQL, ready to use with Docker. Perfect for building applications that require QR codes, Excel spreadsheet processing, database integration, and more.

## 🎯 What is this project for?

**ilocaldev** provides a **plug-and-play** local development environment that includes:

- **🔧 PHP 8.2** with Apache optimized for development
- **🗄️ MySQL 8.0** with data persistence
- **⚙️ phpMyAdmin** for visual database management
- **📊 Pre-installed extensions** for QR codes, Excel handling, image processing, and more
- **🚀 Optimized settings** tailored for modern PHP applications

### 💼 Ideal Use Cases:

- ✅ **QR Code** applications (attendance tracking, registration, inventory)
- ✅ **School or enterprise management** systems
- ✅ Applications with **Excel handling** (importing/exporting datasets)
- ✅ **REST APIs** backed by MySQL
- ✅ **Authentication and session** management systems
- ✅ Applications with **image processing**

## 🛠️ Included Technologies

| Service | Version | Port |
|---------|---------|------|
| **PHP** | 8.2 with Apache | 8080 |
| **MySQL** | 8.0 | 33061 |
| **phpMyAdmin** | Latest | 8081 |

### 📦 Pre-installed PHP Extensions:

- `mysqli`, `pdo_mysql` - MySQL database connectivity
- `gd` - Image processing and QR code generation
- `zip` - Zip archive manipulation and Excel handling (PhpSpreadsheet)
- `mbstring` - Multibyte string support
- `intl` - Internationalization (dates, numbers, currencies)
- `exif` - Image metadata extraction
- `opcache` - PHP performance caching
- `bcmath`, `soap`, `xml` - Arbitrary precision mathematics and web services

## 🚀 Quick Start

### Prerequisites:
- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running

### 1. Clone the repository:
```bash
git clone https://github.com/your-username/ilocaldev.git
cd ilocaldev
```

### 2. Start the environment:
```bash
docker-compose up -d --build
```

### 3. Access your services:
- 🌐 **Web Application**: http://localhost:8080
- 💾 **phpMyAdmin**: http://localhost:8081
- 🗄️ **MySQL Database**: localhost:33061

## 🏗️ Project Structure

```
ilocaldev/
├── docker-compose.yml    # Docker services orchestration
├── php/
│   ├── Dockerfile       # Custom PHP image definition
│   └── php.ini          # Optimized PHP configuration
├── src/                 # 📁 Put your PHP application code here
│   └── index.html       # Sample entry point
└── README.md
```

## 🔧 Database Configuration

### Default credentials:

```bash
# MySQL Configuration
Host: db                    # (inside Docker network)
Host: localhost:33061       # (from your host machine)
User: mi_usuario_db
Password: root
Database: mi_base_de_datos
Root User: root
Root Password: Hholamundo256@
```

### PHP PDO Connection Example:
```php
<?php
// PDO Connection example
try {
    $pdo = new PDO('mysql:host=db;dbname=mi_base_de_datos', 'mi_usuario_db', 'root');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "✅ Successfully connected to MySQL";
} catch(PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}
?>
```

## 📝 Development Workflow

### 1. Adding your code:
Place all your PHP files inside the `src/` directory:
```
src/
├── index.php
├── config/
├── includes/
└── your-app/
```

### 2. Managing dependencies with Composer:
```bash
# Enter the web container
docker-compose exec web bash

# Inside the container
composer install
composer require phpoffice/phpspreadsheet  # For Excel manipulation
composer require chillerlan/php-qr-code    # For QR code generation
```

### 3. Viewing logs in real time:
```bash
docker-compose logs -f web    # PHP / Apache logs
docker-compose logs -f db     # MySQL logs
```

## 🎛️ Useful Commands

### Container management:
```bash
# Check container status
docker-compose ps

# Stop all containers
docker-compose down

# Rebuild containers after Dockerfile or configuration changes
docker-compose up -d --build

# Remove all containers and volumes (CAUTION: wipes database data!)
docker-compose down -v
```

### Accessing containers:
```bash
# Enter PHP/Apache container terminal
docker-compose exec web bash

# Access MySQL CLI directly
docker-compose exec db mysql -u root -p
```

### Database backup and restore:
```bash
# Export / dump database
docker-compose exec db mysqldump -u root -p mi_base_de_datos > backup.sql

# Import database dump
docker-compose exec -T db mysql -u root -p mi_base_de_datos < backup.sql
```

## ⚡ Optimized Configuration

### Increased limits for development:
- **Memory limit**: 512MB
- **Upload max filesize**: 128MB
- **Post max size**: 128MB
- **Max execution time**: 120 seconds
- **Timezone**: America/Bogota

### Applying configuration changes:
Edit `php/php.ini` and rebuild containers:
```bash
docker-compose up -d --build
```

## 🌍 Production Considerations

For production deployments, update the following values in `php/php.ini`:
```ini
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
display_errors = Off
display_startup_errors = Off
opcache.validate_timestamps = 0
```

## 🤝 Contributing

1. Fork the repository
2. Create your branch: `git checkout -b feature/new-feature`
3. Commit your changes: `git commit -m 'Add new feature'`
4. Push to the branch: `git push origin feature/new-feature`
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License. See `LICENSE` for details.

## 🆘 Support

Encountering issues? Please open an [issue](https://github.com/your-username/ilocaldev/issues)!

---

⭐ **If you find this project helpful, give it a star!** ⭐
