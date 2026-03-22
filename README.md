````md
# YouTube LMS (Laravel)

Aplicación web tipo **LMS** (cursos, carrito, checkout con Stripe, paneles de administrador e instructor, usuarios) construida con **Laravel 11**, **Vite**, **Tailwind** y **Alpine.js**. El front público usa plantillas HTML con assets en `public/frontend/`; el panel usa assets en `public/backend/`.

## Requisitos

- PHP **8.2+** (extensiones típicas: `openssl`, `pdo`, `mbstring`, `tokenizer`, `xml`, `ctype`, `json`, `fileinfo`, `curl` y, según la base de datos, `pdo_sqlite` y/o `pdo_mysql`)
- **Composer** 2.x
- **Node.js** LTS y **npm**
- Base de datos: **SQLite** (desarrollo rápido) o **MySQL/MariaDB** (producción habitual)

## Instalación (Linux / macOS)

```bash
git clone <url-de-tu-repo>.git
cd YouTubeLMS
composer install
npm install
cp .env.example .env
php artisan key:generate
````

Configura la base de datos en `.env`:

* **SQLite:** deja `DB_CONNECTION=sqlite` y asegúrate de que exista el archivo `database/database.sqlite` (puede ser un archivo vacío antes de migrar).
* **MySQL:** define `DB_CONNECTION=mysql`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD` y crea la base vacía.

Luego:

```bash
php artisan migrate
php artisan storage:link   # si usas subidas a storage/app/public
```

## Arranque en desarrollo

El script `composer dev` levanta tres procesos: servidor PHP, worker de colas y Vite.

```bash
composer run dev
```

Abre la aplicación en `http://127.0.0.1:8000` (o el puerto que indique `php artisan serve`).

Vite suele mostrar `http://localhost:5173`; en desarrollo con Laravel sirve los assets con *hot reload*. La aplicación se usa en el puerto `8000`, no solo en el `5173`.

Si usas `.env.local` (por ejemplo con `APP_ENV=local`), debe contener el mismo `APP_KEY` que `.env`, o el servidor de desarrollo puede fallar con error de cifrado. Alternativa:

```bash
php artisan serve --no-reload
```

(comportamiento distinto del entorno del subproceso).

## Windows (PowerShell o CMD)

Mismos pasos, con rutas adecuadas. Ejemplo:

```bash
cd C:\ruta\YouTubeLMS
composer install
npm install
copy .env.example .env
php artisan key:generate
php artisan migrate
composer run dev
```

Requisitos: PHP y Composer en el `PATH` (XAMPP/Laragon/instalación manual). En algunos entornos hace falta habilitar extensiones en `php.ini` (`extension=pdo_sqlite`, `extension=pdo_mysql`, etc.).

## Variables de entorno opcionales

* **Stripe:** `STRIPE_TEST_PK`, `STRIPE_TEST_SK` (y configuración en panel admin).
* **Google OAuth:** `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GOOGLE_CALLBACK_REDIRECTS`.
* **Colas:** con `QUEUE_CONNECTION=database` debe existir la tabla `jobs` (incluida en migraciones).

## Comandos útiles

| Comando                     | Uso                                    |
| --------------------------- | -------------------------------------- |
| php artisan serve           | Solo servidor web                      |
| php artisan queue:listen    | Procesar jobs en segundo plano         |
| npm run dev / npm run build | Assets Vite en desarrollo / producción |
| php artisan migrate         | Aplicar migraciones                    |


