Lab2Login 2.0 — Autenticación con Laravel Breeze
Resumen

Este proyecto implementa un flujo de autenticación listo para usar con Laravel Breeze, integrando vistas Blade y estilos con Tailwind. Incluye rutas protegidas, validación, persistencia de sesiones y una estructura MVC clara para continuar el desarrollo del laboratorio.

Tecnologías

PHP 8.2+

Laravel 12

MySQL (WAMP) o SQLite

Composer

Node.js y npm (Vite, Tailwind)

Git

Requisitos previos

WAMP instalado y en ejecución (Apache y MySQL).

PHP CLI disponible en PATH.

Composer instalado.

Node.js 18+ y npm.

Git instalado.

Clonado del repositorio
git clone https://github.com/Val233100/Lab2Login2.0.git
cd Lab2Login2.0

Dependencias PHP
composer install

Variables de entorno y clave de aplicación

Crear el archivo .env desde el ejemplo:

copy .env.example .env


Generar la clave de la aplicación:

php artisan key:generate

Configuración de base de datos
Opción A: MySQL (recomendada con WAMP)

Crear una base de datos en phpMyAdmin con nombre lab2login y cotejamiento utf8mb4_unicode_ci.

Configurar el archivo .env:

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lab2login
DB_USERNAME=root
DB_PASSWORD=


Opcional (sesiones en base de datos):

SESSION_DRIVER=database


Generar la tabla de sesiones si usas SESSION_DRIVER=database:

php artisan session:table

Opción B: SQLite

Crear el archivo de base de datos:

type NUL > database\database.sqlite


Configurar .env:

DB_CONNECTION=sqlite


Opcional (sesiones en base de datos):

php artisan session:table

Migraciones
php artisan migrate

Nota: error 1071 (longitudes de índice en MySQL)

Si aparece Specified key was too long; max key length is 1000 bytes, establece la longitud por defecto en app/Providers/AppServiceProvider.php:

use Illuminate\Support\Facades\Schema;

public function boot(): void
{
    Schema::defaultStringLength(191);
}


Luego recrea las tablas si es necesario:

php artisan migrate:fresh

Dependencias frontend y assets

Instalar dependencias:

npm install


Modo desarrollo (Vite):

npm run dev


Vite servirá assets en http://localhost:5173.

Compilación para producción:

npm run build

Servidor de desarrollo de Laravel

Ejecutar el servidor de Laravel:

php artisan serve


La aplicación quedará disponible en http://localhost:8000.

Rutas relevantes

Registro: /register

Inicio de sesión: /login

Panel autenticado: /dashboard

Perfil: /profile

Estructura del proyecto (MVC)
Modelos

app/Models/User.php: entidad de usuario (autenticación, notificaciones).

Vistas (Blade)

Autenticación: resources/views/auth/*

Layouts: resources/views/layouts/*

Dashboard: resources/views/dashboard.blade.php

Controladores

Autenticación: app/Http/Controllers/Auth/*

Perfil: app/Http/Controllers/ProfileController.php

Rutas

Públicas y autenticación: routes/auth.php

Web y rutas protegidas: routes/web.php

Migraciones

database/migrations/*: usuarios, caché, jobs y (opcional) sesiones.

Middlewares

auth: restringe acceso a usuarios autenticados.

guest: restringe a invitados (login/register).

verified: para verificación de email si se habilita.

Comandos útiles
php artisan route:list
php artisan migrate:status
php artisan migrate:fresh
php artisan tinker
php artisan cache:clear
php artisan config:clear

Resultado esperado

Acceso a /register para crear cuenta.

Acceso a /login para iniciar sesión.

Redirección a /dashboard al autenticarse.

Edición de perfil en /profile.

Integración con WAMP (Apache)

Alternativa a php artisan serve: configurar un VirtualHost que apunte a public/.

Ejemplo (httpd-vhosts.conf):

<VirtualHost *:80>
    ServerName lab2login.local
    DocumentRoot "C:/wamp64/www/Lab2Login/public"
    <Directory "C:/wamp64/www/Lab2Login/public">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>


Editar el archivo hosts:

127.0.0.1  lab2login.local


Asegúrate de tener habilitado mod_rewrite y AllowOverride All para que .htaccess funcione correctamente.

Buenas prácticas

No versionar .env ni storage/* (ya contemplado en .gitignore).

Ejecutar composer install y npm install después de clonar.

Usar migraciones en lugar de crear tablas manualmente.

Mantener APP_DEBUG=false en producción.

Validar entradas con Form Requests y reglas en controladores.

Solución de problemas

vendor/autoload.php no existe
Causa: no se han instalado dependencias.
Solución: ejecutar composer install dentro del directorio del proyecto.

Could not open input file: artisan
Causa: ejecutando el comando fuera del proyecto.
Solución: ubicarse en la carpeta del proyecto (debe existir el archivo artisan).

Specified key was too long; max key length is 1000 bytes (1071)
Causa: límites de índice en MySQL.
Solución: Schema::defaultStringLength(191) en AppServiceProvider y php artisan migrate:fresh.

Base table or view already exists en migraciones
Causa: tablas existentes.
Solución: revisar con php artisan migrate:status o recrear con php artisan migrate:fresh.

Sesiones en base de datos faltan
Causa: SESSION_DRIVER=database sin tabla.
Solución: php artisan session:table && php artisan migrate.

PowerShell bloquea npm o scripts
Solución:

Set-ExecutionPolicy -Scope CurrentUser RemoteSigned

Capturas de pantalla

Colocar evidencias en docs/capturas/:

login.png: pantalla de inicio de sesión.

register.png: formulario de registro.

dashboard.png: vista autenticada.

profile.png: edición de perfil.
