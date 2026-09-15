# 🌐 Internet Programs — Laravel Web Application

<div align="center">

![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4-38BDF8?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-7.x-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-Storage-FF9900?style=for-the-badge&logo=amazons3&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)

**A full-stack Laravel 12 web application featuring JWT authentication, Google OAuth, cloud file storage, Excel/PDF reporting, activity logging, and a PM2-managed multi-instance deployment.**

[Getting Started](#getting-started) · [Tech Stack](#tech-stack) · [Deployment](#deployment)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Deployment](#deployment)
- [Project Structure](#project-structure)

---

## Overview

A modern Laravel 12 web application built on PHP 8.2, featuring a clean API-first backend with dual authentication (JWT stateless + Sanctum session), cloud storage via AWS S3, comprehensive reporting (Excel & PDF export), full activity auditing, and a multi-process PM2 deployment that distributes traffic across four independent PHP workers for local load-balancing.

---

## Features

### 🔐 Authentication & Security
- **JWT authentication** (stateless, token-based) via `tymon/jwt-auth`
- **Laravel Sanctum** for SPA / session-based API consumers
- **Google OAuth** sign-in via `google/auth`
- BCrypt password hashing with configurable rounds

### 📁 File & Media Management
- Cloud file storage on **AWS S3** with Laravel's Flysystem adapter
- Image upload and processing (resize, crop, optimize) via `intervention/image`
- Local disk fallback for development

### 📊 Reporting & Exports
- **Excel export/import** with `maatwebsite/excel` (XLSX, CSV)
- **PDF generation** from Blade templates via `barryvdh/laravel-dompdf`
- Downloadable reports with dynamic data

### 📧 Email
- Transactional email via **MailerSend** (`mailersend/laravel-driver`)
- Blade-based email templates
- Queue-backed sending for non-blocking delivery

### 📋 Activity Logging & Audit Trail
- Full audit log of user actions via `spatie/laravel-activitylog`
- Tracks model changes, logins, and custom events
- Queryable log with filtering by subject, causer, and date

### ⚡ Performance
- **Laravel Octane** for high-throughput request handling
- **PM2** process manager running four parallel PHP workers (ports 8000–8003)
- Queue-backed jobs for heavy processing

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | Laravel 12 |
| **Language** | PHP 8.2 |
| **Frontend** | Tailwind CSS v4 + Vite 7 |
| **Auth** | JWT (`tymon/jwt-auth`) + Laravel Sanctum |
| **OAuth** | Google (`google/auth`) |
| **Storage** | AWS S3 (Flysystem v3) |
| **Image** | Intervention Image v3 |
| **Excel** | Maatwebsite Excel v3 |
| **PDF** | Laravel DomPDF v3 |
| **Email** | MailerSend |
| **Logging** | Spatie Activity Log v4 |
| **Jobs** | Laravel Queue (database driver) |
| **Performance** | Laravel Octane |
| **Process Manager** | PM2 (4 worker instances) |
| **Dev Environment** | WAMP (Windows, Apache, MySQL, PHP) |
| **Testing** | PHPUnit 11 |

---

## Getting Started

### Prerequisites

- PHP 8.2+
- Composer
- Node.js 18+ and npm
- A running MySQL or SQLite database

### 1 — Clone and install

```bash
git clone https://github.com/abdurrahmanalzoubi8-stack/internet-programs.git
cd internet-programs
composer install
npm install
```

### 2 — Configure environment

```bash
cp .env.example .env
php artisan key:generate
php artisan jwt:secret
```

Edit `.env` with your database credentials, AWS keys, and MailerSend settings.

### 3 — Run migrations

```bash
php artisan migrate
```

### 4 — Build assets and start

```bash
# Option A — single dev server
npm run build
php artisan serve

# Option B — full dev mode with hot reload
composer run dev
```

---

## Environment Variables

| Variable | Description |
|---|---|
| `APP_KEY` | Application encryption key |
| `DB_CONNECTION` | Database driver (`sqlite` default, `mysql` for production) |
| `JWT_SECRET` | JWT signing secret (`php artisan jwt:secret`) |
| `AWS_ACCESS_KEY_ID` | AWS credentials for S3 storage |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key |
| `AWS_BUCKET` | S3 bucket name |
| `AWS_DEFAULT_REGION` | S3 bucket region |
| `MAIL_MAILER` | Mail driver (set to `mailersend` in production) |
| `MAIL_FROM_ADDRESS` | Sender email address |

---

## Deployment

### With PM2 (multi-instance, Windows)

The `ecosystem.config.cjs` file launches four independent PHP workers managed by PM2:

```bash
# Install PM2 globally
npm install -g pm2

# Start all four instances
pm2 start ecosystem.config.cjs

# Monitor processes
pm2 monit

# View logs
pm2 logs

# Stop all
pm2 stop all
```

This runs the application on **ports 8000 – 8003**, allowing an upstream reverse proxy (Nginx or Apache) to load-balance across all four workers.

| Process | Port |
|---|---|
| `laravel-8000` | http://127.0.0.1:8000 |
| `laravel-8001` | http://127.0.0.1:8001 |
| `laravel-8002` | http://127.0.0.1:8002 |
| `laravel-8003` | http://127.0.0.1:8003 |

### Stopping the cluster

```bash
pm2 stop all
# or
pm2 delete all
```

### Running tests

```bash
composer run test
# or directly:
php artisan config:clear && php artisan test
```

---

## Project Structure

```
app/
├── Http/
│   ├── Controllers/     # Request handling
│   └── Middleware/      # Auth, throttle, etc.
├── Models/              # Eloquent models
└── ...
database/
├── migrations/          # Schema definitions
└── seeders/             # Seed data
resources/
├── views/               # Blade templates
└── js/ & css/           # Frontend source (Vite)
routes/
├── web.php              # Web routes
└── api.php              # API routes
tests/                   # PHPUnit test suite
ecosystem.config.cjs     # PM2 multi-instance config
```

---

## License

MIT
