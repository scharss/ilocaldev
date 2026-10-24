# 🐳 ilocaldev - PHP Development Environment with Docker

Un entorno de desarrollo completo y optimizado para aplicaciones PHP con MySQL, listo para usar con Docker. Perfecto para desarrollar aplicaciones que requieren códigos QR, manejo de Excel, bases de datos y más.

## 🎯 ¿Para qué sirve este proyecto?

Este proyecto proporciona un entorno de desarrollo **plug-and-play** que incluye:

- **🔧 PHP 8.3.36** con Apache optimizado para desarrollo
- **🗄️ MySQL 8.0** con persistencia de datos
- **⚙️ phpMyAdmin** para gestión visual de bases de datos
- **📊 Extensiones pre-instaladas** para QR codes, Excel, imágenes y más
- **🚀 Configuración optimizada** para aplicaciones modernas PHP

### 💼 Casos de uso ideales

Este entorno sirve para **Desarrollo Web** en general, no solo para una aplicación concreta. Puedes tener varios proyectos conviviendo en `src/`.

**Desarrollo Web**
- ✅ **Desarrollo Web** de sitios corporativos, blogs, landing pages, portafolios, intranets y aplicaciones a medida

**CMS**
- ✅ WordPress: temas, plugins y multisitio

**Aplicaciones y utilidades**
- ✅ Sistemas de QR/NFC, asistencia, inventario y trazabilidad
- ✅ Gestión escolar, empresarial, clientes, citas y reportes
- ✅ Importación/exportación de Excel/CSV y generación de reportes
- ✅ Autenticación, sesiones, roles y paneles administrativos
- ✅ Procesamiento de imágenes, generación de códigos QR y archivos ZIP
- ✅ Migración de proyectos PHP legacy y pruebas locales
- ✅ Laboratorio multi-proyecto: varias webs y bases de datos en el mismo entorno

> **Compatibilidad:** ilocaldev está pensado para aplicaciones PHP servidas desde la raíz del proyecto. Frameworks o CMS que requieran `DocumentRoot` en `public/` necesitan configuración adicional de Apache.

## 🛠️ Tecnologías Incluidas

| Servicio | Versión | Puerto |
|----------|---------|--------|
| **PHP** | 8.3.36 con Apache | 8080 |
| **MySQL** | 8.0 | 33061 |
| **phpMyAdmin** | 5.2.3 con PHP 8.3.36 | 8081 |

### 📦 Extensiones PHP pre-instaladas:

- `mysqli`, `pdo_mysql` - Conexión a bases de datos
- `gd` - Procesamiento de imágenes y códigos QR
- `zip` - Manejo de archivos comprimidos y Excel
- `mbstring` - Soporte para caracteres multibyte
- `intl` - Internacionalización (fechas, números)
- `exif` - Metadatos de imágenes
- `opcache` - Cache de rendimiento
- `bcmath`, `soap`, `xml` - Matemáticas y servicios web

### 🧰 Herramientas incluidas

- Composer 2.x
- Git
- curl
- zip/unzip
- Apache con `mod_rewrite`
- OPcache

## 🚀 Instalación Rápida

### Requisitos previos:
- [Docker Desktop](https://www.docker.com/products/docker-desktop) instalado

### 1. Clonar el repositorio:
```bash
git clone https://github.com/scharss/ilocaldev.git

```

### 2. Iniciar el entorno:
```bash
docker-compose up -d --build
```

## 🐘 Versión de PHP configurable

La versión de PHP del contenedor web se define en `.env`:

```env
PHP_VERSION=8.3.36
```

Después de cambiarla, reconstruye y recrea el contenedor web:

```bash
docker compose build --pull web
docker compose up -d --no-deps --force-recreate web
```

La versión mínima aceptada es la que incluye los parches de CVE-2026-109618 y CVE-2026-109619: **8.2.35, 8.3.36, 8.4.27 o 8.5.12**.

### 3. ¡Listo! Accede a:
- 🌐 **Tu aplicación**: http://localhost:8080
- 💾 **phpMyAdmin**: http://localhost:8081
- 🗄️ **MySQL**: localhost:33061

## 🏗️ Estructura del Proyecto

```
proyecto/
├── docker-compose.yml    # Configuración de servicios Docker
├── php/
│   ├── Dockerfile       # Imagen PHP personalizada
│   └── php.ini          # Configuración PHP optimizada
├── src/                 # 📁 Aquí va tu código PHP
│   └── index.php        # Archivo de ejemplo
└── README.md
```

## 🔧 Configuración de Base de Datos

### Credenciales:

Las credenciales de MySQL se definen en `.env`:

```env
MYSQL_ROOT_PASSWORD=...
MYSQL_DATABASE=...
MYSQL_USER=...
MYSQL_PASSWORD=...
```

> `docker compose config` resuelve las variables y muestra las contraseñas. No compartas su salida. Para validar sin imprimir nada usa `docker compose config --quiet`.

Host: `db` (dentro de Docker)  
Host: `localhost:33061` (desde tu PC)

### Conexión desde PHP:
```php
<?php
// Ejemplo de conexión PDO
try {
    $pdo = new PDO('mysql:host=db;dbname=mi_base_de_datos', 'mi_usuario_db', 'TU_PASSWORD');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "✅ Conexión exitosa a MySQL";
} catch(PDOException $e) {
    echo "❌ Error: " . $e->getMessage();
}
?>
```

## 📝 Desarrollo

### 1. Colocar tu código:
Coloca todos tus archivos PHP en la carpeta `src/`:
```
src/
├── index.php
├── config/
├── includes/
└── tu-aplicacion/
```

### 2. Instalar dependencias con Composer:
```bash
# Entrar al contenedor
docker-compose exec web bash

# Dentro del contenedor
composer install
composer require phpoffice/phpspreadsheet  # Para Excel
composer require chillerlan/php-qr-code    # Para códigos QR
```

### 3. Ver logs en tiempo real:
```bash
docker-compose logs -f web    # Logs de PHP/Apache
docker-compose logs -f db     # Logs de MySQL
```

## 🎛️ Comandos Útiles

### Administración de contenedores:
```bash
# Ver estado de contenedores
docker-compose ps

# Detener todo
docker-compose down

# Reconstruir después de cambios
docker-compose up -d --build

# Limpiar todo (¡ELIMINA DATOS!)
docker-compose down -v
```

### Acceso a contenedores:
```bash
# Entrar al contenedor web (PHP)
docker-compose exec web bash

# Entrar al contenedor de MySQL
docker-compose exec db mysql -u root -p
```

### Backup de base de datos:
```bash
# Exportar base de datos
docker-compose exec db mysqldump -u root -p mi_base_de_datos > backup.sql

# Importar base de datos
docker-compose exec -T db mysql -u root -p mi_base_de_datos < backup.sql
```

## ⚡ Configuración Optimizada

### Límites aumentados para desarrollo:
- **Memory limit**: 512MB
- **Upload max**: 128MB
- **Post max**: 128MB
- **Execution time**: 120 segundos
- **Timezone**: America/Bogota

### Para cambiar configuraciones:
Edita `php/php.ini` y reconstruye:
```bash
docker-compose up -d --build
```

## 🌍 Para Producción

Para usar en producción, modifica en `php/php.ini`:
```ini
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
display_errors = Off
display_startup_errors = Off
opcache.validate_timestamps = 0
```

## 🤝 Contribuir

1. Fork del proyecto
2. Crea tu rama: `git checkout -b feature/nueva-funcionalidad`
3. Commit: `git commit -m 'Agregar nueva funcionalidad'`
4. Push: `git push origin feature/nueva-funcionalidad`
5. Abre un Pull Request

## 📜 Licencia

Este proyecto está bajo la Licencia MIT. Ver `LICENSE` para más detalles.

## 🆘 Soporte

¿Problemas? ¡Abre un [issue](https://github.com/tu-usuario/php-docker-dev/issues)!

---

⭐ **¡Si te gusta este proyecto, dale una estrella!** ⭐


; Para producción cambiar a:
error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
display_errors = Off
display_startup_errors = Off
opcache.validate_timestamps = 0












