# Docker Dev Environment | محیط توسعه داکری

<div dir="rtl">

یک محیط توسعه کامل مبتنی بر Docker برای پروژه‌های PHP با پشتیبانی از چندین نسخه PHP، سرور Nginx و پایگاه داده MariaDB.

</div>

A complete Docker-based development environment for PHP projects with multi-version PHP support, Nginx web server, and MariaDB database.

---

<div dir="rtl">

## ویژگی‌ها

- **چند نسخه PHP**: پشتیبانی از PHP 7.4، 8.1 و 8.2
- **سرور وب**: Nginx به عنوان سرور وب
- **پایگاه داده**: MariaDB 10.11
- **Composer**: Composer 2 به صورت پیش‌فرض در همه کانتینرهای PHP نصب شده
- **انعطاف‌پذیری**: امکان اضافه کردن پروژه‌های جدید به سادگی

</div>

## Features

- **Multi-version PHP**: Support for PHP 7.4, 8.1, and 8.2
- **Web Server**: Nginx as the web server
- **Database**: MariaDB 10.11
- **Composer**: Composer 2 pre-installed in all PHP containers
- **Flexible**: Easy to add new projects

---

<div dir="rtl">

## ساختار پروژه

</div>

## Project Structure

```
docker-dev/
├── docker-compose.yml
├── php74/
│   └── Dockerfile
├── php81/
│   └── Dockerfile
├── php82/
│   └── Dockerfile
└── nginx/
    └── conf.d/
        ├── avin-project.conf
        ├── diako-project.conf
        └── dana-project.conf
```

---

<div dir="rtl">

## پیش‌نیازها

</div>

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (v20.10+)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2+)

---

<div dir="rtl">

## شروع سریع

</div>

## Quick Start

### ۱. کلون کردن مخزن

```bash
git clone <repository-url>
cd docker-dev
```

### ۲. اجرای سرویس‌ها

```bash
docker compose up -d
```

### ۳. بررسی وضعیت کانتینرها

```bash
docker compose ps
```

---

<div dir="rtl">

## پورت‌های سرویس‌ها

</div>

## Service Ports

| سرویس / Service | پورت / Port | توضیح / Description |
|---|---|---|
| MariaDB | 3306 | پایگاه داده / Database |
| Nginx (avin) | 8001 | پروژه avin با PHP 8.1 |
| Nginx (diako) | 8002 | پروژه diako با PHP 8.1 |
| Nginx (dana) | 8003 | پروژه dana با PHP 8.2 |

---

<div dir="rtl">

## اضافه کردن پروژه جدید

</div>

## Adding a New Project

### ۱. ایجاد فایل پیکربندی Nginx

یک فایل جدید در مسیر `nginx/conf.d/` با نام `my-project.conf` بسازید:

```nginx
server {
    listen 8004;

    server_name localhost;

    root /var/www/my-project/public;

    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php81:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

<div dir="rtl">

### ۲. قرار دادن فایل‌های پروژه

فایل‌های پروژه خود را در مسیر `D:/projects/my-project/` قرار دهید.

</div>

### ۳. ری‌استارت کانتینرها

```bash
docker compose restart nginx
```

---

<div dir="rtl">

## پورت‌های قابل دسترسی

برای اضافه کردن پورت جدید، محدوده پورت‌ها را در `docker-compose.yml` تغییر دهید:

```yaml
ports:
  - "8000-8030:8000-8030"
```

</div>

---

<div dir="rtl">

## مدیریت پایگاه داده

</div>

## Database Management

### اتصال به MariaDB

```bash
docker exec -it mariadb mariadb -u root -prootsecret
```

### ایجاد دیتابیس جدید

```sql
CREATE DATABASE my_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

---

<div dir="rtl">

## دستورات مفید

</div>

## Useful Commands

| دستور / Command | توضیح / Description |
|---|---|
| `docker compose up -d` | اجرای سرویس‌ها در حالت پس‌زمینه |
| `docker compose down` | توقف و حذف کانتینرها |
| `docker compose logs -f` | نمایش زنده لاگ‌ها |
| `docker compose restart` | ری‌استارت کانتینرها |
| `docker compose exec php81 bash` | ورود به کانتینر PHP 8.1 |
| `docker compose build --no-cache` | بازسازی تصاویر Docker |

---

<div dir="rtl">

## نکات مهم

- پروژه‌های شما باید در مسیر `D:/projects/` قرار داشته باشند تا به کانتینرها مونت شوند.
- پورت MariaDB (3306) ممکن است با سرویس‌های دیگر سیستم تداخل داشته باشد. در صورت نیاز آن را در `docker-compose.yml` تغییر دهید.
- رمز root ماریادب: `rootsecret` (فقط برای محیط توسعه)

</div>

## Important Notes

- Your projects must be placed in the `D:/projects/` directory to be mounted into containers.
- The MariaDB port (3306) may conflict with other services on your system. Change it in `docker-compose.yml` if needed.
- MariaDB root password: `rootsecret` (development only)

---

<div dir="rtl">

## لایسنس

این پروژه برای استفاده شخصی و توسعه طراحی شده است.

</div>

## License

This project is designed for personal use and development purposes.
