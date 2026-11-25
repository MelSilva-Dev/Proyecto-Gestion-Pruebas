# U-Ride — Gestión de viajes compartidos

Aplicación web para coordinar viajes compartidos entre estudiantes. U-Ride reúne publicación de viajes, solicitudes de cupo, perfiles, calificaciones y reportes en una interfaz conectada a una API REST con autenticación y actualizaciones en tiempo real.

## Funcionalidades

- Registro, inicio de sesión y recuperación de acceso.
- Publicación y consulta de viajes por zona, destino, fecha y cupos.
- Solicitudes de reserva, gestión de estados y notificaciones en tiempo real.
- Perfiles, calificaciones y reportes con vistas de administración.
- Interfaz adaptable a escritorio y móvil.
- Documentación interactiva de la API con Swagger.

## Tecnologías

| Componente | Tecnologías |
| --- | --- |
| Frontend | React, TypeScript, Vite, React Router, TanStack Query, Zustand |
| Backend | Node.js, NestJS, TypeScript, Socket.IO |
| Persistencia | PostgreSQL, TypeORM |
| Validación y autenticación | class-validator, class-transformer, JWT, bcrypt |
| Contenedores | Docker Compose |

## Estructura del proyecto

```text
.
├── back-end/       # API REST, autenticación y WebSockets
├── front-end/      # Aplicación web React
└── docker-compose.yml
```

## Requisitos

- Node.js 20 o superior y pnpm.
- Docker Desktop con Docker Compose, para ejecutar la aplicación completa.

## Inicio rápido con Docker

Desde la raíz del repositorio, ejecuta:

```bash
docker compose up --build
```

Al terminar la compilación, abre:

- Aplicación web: [http://localhost:8080](http://localhost:8080)
- API: [http://localhost:3000](http://localhost:3000)
- Swagger: [http://localhost:3000/api](http://localhost:3000/api)
- PostgreSQL: `localhost:5454`

Para detener los servicios, pulsa `Ctrl+C`; también puedes ejecutar `docker compose down`. Los datos de PostgreSQL se conservan en el volumen `postgres_data`.

## Desarrollo local

### 1. Base de datos

Inicia PostgreSQL con Docker Compose:

```bash
docker compose up -d db
```

La configuración de desarrollo incluida en Compose crea la base `g_pruebas` y la expone en el puerto `5454`.

### 2. Backend

En una terminal:

```bash
cd back-end
pnpm install
```

Crea `back-end/.env` con la configuración de tu entorno:

```dotenv
PORT=3000
DB_HOST=localhost
DB_PORT=5454
DB_USERNAME=pruebas
DB_PASSWORD=<tu-contraseña-local>
DB_NAME=g_pruebas
DB_SYNCHRONIZE=true
DB_SSL=false
JWT_SECRET=<secreto-local-largo-y-aleatorio>
```

Inicia el servidor en modo desarrollo:

```bash
pnpm run start:dev
```

### 3. Frontend

En otra terminal:

```bash
cd front-end
pnpm install
```

Crea `front-end/.env`:

```dotenv
VITE_API_BASE_URL=http://localhost:3000
```

Inicia Vite:

```bash
pnpm run dev
```

Vite mostrará en la terminal la dirección local de la aplicación, normalmente [http://localhost:5173](http://localhost:5173).

## Variables de entorno

| Variable | Componente | Descripción |
| --- | --- | --- |
| `PORT` | Backend | Puerto HTTP de la API (predeterminado: `3000`). |
| `DB_HOST` | Backend | Host de PostgreSQL; usa `db` en Compose y `localhost` en desarrollo local. |
| `DB_PORT` | Backend | Puerto de PostgreSQL; usa `5432` dentro de Compose y `5454` desde el equipo local. |
| `DB_USERNAME` | Backend | Usuario de PostgreSQL. |
| `DB_PASSWORD` | Backend | Contraseña de PostgreSQL. |
| `DB_NAME` | Backend | Nombre de la base de datos. |
| `DB_SYNCHRONIZE` | Backend | Sincroniza entidades con el esquema al iniciar. En producción se recomienda `false` y migraciones. |
| `DB_SSL` | Backend | Habilita la conexión TLS a PostgreSQL cuando está en `true`. |
| `JWT_SECRET` | Backend | Secreto usado para firmar tokens; define uno privado y seguro para cada entorno. |
| `VITE_API_BASE_URL` | Frontend | URL base de la API. Debe ser accesible desde el navegador. |
| `VITE_GOOGLE_MAPS_API_KEY` | Frontend | Clave opcional para las funciones de mapas. |
| `VITE_TOKEN_PAYPHONE` | Frontend | Token opcional para la integración de pagos Payphone. |
| `VITE_STORE_ID` | Frontend | Identificador de tienda Payphone. |

No publiques archivos `.env`, credenciales ni claves privadas en el repositorio. Los valores incluidos en `docker-compose.yml` son únicamente para desarrollo local: reemplázalos y administra los secretos de forma segura antes de cualquier despliegue.

## API

La especificación OpenAPI y la interfaz Swagger están disponibles en `/api` cuando el backend está en ejecución. Los módulos principales son:

- `/auth`: registro, inicio de sesión y recuperación de acceso.
- `/users`: consulta y administración de usuarios y perfiles.
- `/trips`: creación, consulta, actualización y ciclo de vida de viajes.
- `/requests`: solicitudes de reserva y cambios de estado.
- `/ratings`: calificaciones.
- `/reports`: reportes y gestión administrativa.

La API usa JWT para las rutas protegidas. El frontend se conecta al servidor de WebSockets para recibir eventos de solicitudes y reportes.

## Comandos útiles

Backend, desde `back-end/`:

```bash
pnpm run start:dev   # desarrollo con recarga
pnpm run build       # compilación
pnpm run start:prod  # ejecución de la compilación
pnpm test            # pruebas unitarias
pnpm run test:e2e    # pruebas end-to-end
```

Frontend, desde `front-end/`:

```bash
pnpm run dev      # servidor de desarrollo
pnpm run build    # comprobación TypeScript y compilación
pnpm run preview  # vista previa de la compilación
pnpm run lint     # análisis estático
```

## Notas de seguridad y despliegue

- Configura secretos distintos y robustos para cada entorno y no reutilices credenciales locales.
- `DB_SYNCHRONIZE=true` facilita el desarrollo, pero el despliegue debe usar migraciones y mantenerlo desactivado.
- Configura HTTPS, orígenes CORS permitidos y acceso a la base de datos según el entorno de despliegue.
- Protege las claves de servicios externos y restringe las claves de navegador por dominio cuando el proveedor lo permita.

## Licencia

Proyecto académico. No se especifica una licencia de distribución en este repositorio.
