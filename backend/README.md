# Backend — Gestión de Usuarios (Laravel API)

Backend en **Laravel** ejecutado con **Docker Compose** (Nginx + PHP-FPM + MySQL + Redis).

> Este repositorio **ya incluye** el proyecto Laravel en `backend/gestion-usuarios-backend/`.
> **No uses `composer create-project`** después de clonar.

---

## Servicios

- **nginx**: expone la app en `http://localhost:8080`
- **app** (php-fpm): Laravel (dentro del contenedor)
- **db** (MySQL 8): persistencia
- **redis**: cache/colas (opcional por ahora)

## Puertos

- App: `http://localhost:8080`
- MySQL (host): `localhost:3307` (interno docker: `db:3306`)
- Redis (host): `localhost:6380` (interno docker: `redis:6379`)

## Estructura

- Laravel: `backend/gestion-usuarios-backend/`
- Docker:
  - `backend/docker-compose.yml`
  - `backend/docker/nginx/default.conf`
  - `backend/docker/php/Dockerfile`

---

## Setup (después de clonar)

Desde la carpeta `backend/`:

```bash
docker compose up -d --build
docker compose exec app composer install
docker compose exec app cp .env.example .env
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate