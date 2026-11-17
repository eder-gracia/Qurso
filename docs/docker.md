# Entorno Docker de Qurso

Laravel + Nginx + MySQL + phpMyAdmin. Las credenciales de `docker-compose.yml`
son SOLO para desarrollo local; no se usan en producción.

## Requisitos
- Docker Desktop (en Windows, con WSL 2).
- Visual Studio Code.

## Primera vez (solo la primera persona que crea el proyecto Laravel)
```bash
mkdir src
docker compose build
docker compose run --rm app composer create-project laravel/laravel .
```
Después, editar `src/.env`:
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=qurso
DB_USERNAME=qurso
DB_PASSWORD=secret
```
```bash
docker compose up -d
docker compose exec app php artisan migrate
```

## Cuando `src/` ya está en el repositorio (el resto del equipo)
```bash
docker compose build
docker compose run --rm app composer install
cp src/.env.example src/.env        # PowerShell: Copy-Item src\.env.example src\.env
# editar src/.env con las líneas DB_* de arriba
docker compose run --rm app php artisan key:generate
docker compose up -d
docker compose exec app php artisan migrate
```

## Direcciones
- Aplicación: http://localhost:8080
- phpMyAdmin: http://localhost:8081 (usuario `root`, contraseña `root`)

## Comandos útiles
- Parar: `docker compose down` (la base de datos se conserva)
- Parar y borrar también la base de datos: `docker compose down -v`
- Artisan: `docker compose exec app php artisan <comando>`

## Error "tempnam(): file created in the system's temporary directory" / error 500
PHP (usuario `www-data`) no puede escribir en `src/storage` ni `src/bootstrap/cache`,
porque los archivos los creó `root` al instalar Laravel. Solución:
```bash
docker compose exec app chmod -R 777 storage bootstrap/cache
docker compose exec app php artisan optimize:clear
```
Si sigue fallando, mirar el error real en `src/storage/logs/laravel.log`
o con `docker compose logs app`.
