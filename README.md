# TravesIA Back

API REST de TravesIA, una aplicación web de planificación de viajes con IA que convierte las preferencias del usuario en itinerarios día a día. Este repositorio contiene el servidor; el cliente está en [TravesIA-front](https://github.com/UNIRDAW-TravesIA/TravesIA-front).

Proyecto intermodular del ciclo DAW. Todo el código debe ser comprendido y defendible por quien lo integra.

## Stack

- Node.js 24 y Express 5 (módulos ES).
- nodemon para el entorno de desarrollo.
- Despliegue en Render, con deploy automático al hacer merge en `main`.

## Requisitos previos

- [Node.js 24](https://nodejs.org) (incluye npm 11). Comprueba con `node -v`.
- [Git](https://git-scm.com).

## Puesta en marcha

```bash
git clone https://github.com/UNIRDAW-TravesIA/TravesIA-back.git
cd TravesIA-back
git checkout dev
npm install
cp .env.example .env
npm run dev
```

El servidor queda disponible en `http://localhost:3000`.

Ejecuta `npm install` cada vez que cambies de rama o hagas `git pull`, por si alguien ha añadido dependencias.

### Comandos

| Comando | Descripción |
|---|---|
| `npm run dev` | Servidor con recarga automática (nodemon) |
| `npm start` | Servidor para producción |

## Estructura

```
index.js                      Arranque: lee PORT y escucha peticiones
app.js                        Configura Express y monta las rutas
routes/health.routes.js       Endpoint de comprobación de estado
```

Cada grupo de endpoints vive en su propio archivo dentro de `routes/` y se monta en `app.js`.

## Variables de entorno

Se definen en `.env`, que no se sube al repositorio. `.env.example` lista los nombres sin valores reales.

| Variable | Descripción |
|---|---|
| `PORT` | Puerto en el que escucha el servidor. Render lo asigna automáticamente. |

## API

Todas las respuestas son JSON.

| Método | Ruta | Respuesta |
|---|---|---|
| `GET` | `/api/health` | `{ "status": "ok" }` |

Cuando se añadan endpoints, se documentan aquí para que el cliente pueda consumirlos.

## Flujo de trabajo con Git

| Rama | Uso |
|---|---|
| `main` | Producción. Cada merge despliega automáticamente. |
| `dev` | Integración. Aquí se juntan las features antes de pasar a producción. |
| `feature/<nombre>` | Una rama por tarea, creada siempre desde `dev`. |

Nunca se hace push directo a `main` ni a `dev`. Todo entra mediante Pull Request.

1. Actualiza `dev` y crea tu rama:

```bash
git checkout dev
git pull origin dev
git checkout -b feature/nombre-descriptivo
```

2. Trabaja y haz commits pequeños y frecuentes:

```bash
git add <archivos>
git commit -m "feat: añade endpoint de itinerarios"
```

3. Sube la rama:

```bash
git push -u origin feature/nombre-descriptivo
```

4. En GitHub abre un Pull Request con **base `dev`** y **compare tu rama**. Comprueba el desplegable de base antes de crearlo, por defecto GitHub propone `main`.
5. Pide revisión a otro miembro del equipo. El CI debe pasar y debe haber al menos una aprobación.
6. Tras el merge, borra la rama y vuelve a empezar desde `dev` actualizado.

Cuando `dev` esté estable, se abre un Pull Request con **base `main`** y **compare `dev`**. Al hacer merge se dispara el deploy.

### Convención de commits

Formato `tipo: descripción` en español, corto y en imperativo.

| Tipo | Uso |
|---|---|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de un error |
| `refactor` | Cambio interno sin modificar el comportamiento |
| `docs` | Documentación |
| `chore` | Configuración, dependencias y tareas de mantenimiento |

## Convenciones de código

- Documentación y commits en español; identificadores de código en inglés.
- Sin comentarios que expliquen lo obvio. Solo se comenta el porqué de una decisión no evidente.
- Los endpoints que usen un LLM deben forzar una respuesta JSON estricta, sin Markdown ni texto adicional.
- Los secretos van siempre en variables de entorno.
- Si se usa IA como apoyo, hay que entender el código que se integra y saber justificarlo en la demo.

## Reuniones

Sesiones de trabajo los sábados de 11:00 a 13:00. Los primeros 15-20 minutos son para la demo de lo realizado y la explicación técnica de las decisiones.
