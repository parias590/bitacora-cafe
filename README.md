# Bitácora de Café

App web instalable (PWA) para registrar preparaciones de café, valorarlas y recibir sugerencias de receta.
Los datos se guardan en tu móvil y se sincronizan con un archivo `bitacora-cafe.json` en tu Google Drive,
así ves el mismo historial en todos tus dispositivos.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La app completa |
| `config.js` | Aquí pegas tu ID de cliente de Google (paso 6) |
| `manifest.webmanifest`, iconos | Para instalarla en la pantalla de inicio |
| `sw.js` | Hace que funcione sin conexión |

## Puesta en marcha (unos 15 minutos)

### A. Publicar la app en GitHub Pages

1. Entra en https://github.com/new con tu cuenta (parias590).
   - Nombre del repositorio: `bitacora-cafe`
   - Visibilidad: **Public** (GitHub Pages gratis lo necesita; tus cafés no están en el repositorio, están en tu Drive).
   - Pulsa **Create repository**.
2. En el repositorio vacío, pulsa **uploading an existing file**, arrastra **todos los archivos de esta carpeta** (no la carpeta, su contenido) y pulsa **Commit changes**.
3. Ve a **Settings → Pages**. En *Build and deployment*, Source: **Deploy from a branch**, Branch: **main** y carpeta **/ (root)**. Pulsa **Save**.
   A los 1–2 minutos la app estará en: **https://parias590.github.io/bitacora-cafe/**

### B. Permitir que la app use tu Google Drive

4. Entra en https://console.cloud.google.com/ con tu cuenta de Google.
   Arriba, en el selector de proyectos, pulsa **Nuevo proyecto**, ponle de nombre `Bitacora Cafe` y créalo. Asegúrate de que queda seleccionado.
5. Activa la API de Drive: menú **APIs y servicios → Biblioteca**, busca **Google Drive API** y pulsa **Habilitar**.
6. Configura el acceso con Google: menú **APIs y servicios → Pantalla de consentimiento de OAuth** (o **Google Auth Platform**).
   - Pulsa **Comenzar**. Nombre de la app: `Bitácora de Café`. Correo de asistencia: tu Gmail.
   - Público: **Externo**. Correo de contacto: tu Gmail. Acepta y crea.
   - En **Público (Audience) → Usuarios de prueba**, pulsa **Add users** y añade tu Gmail (y el de cualquier otra cuenta que vayas a usar).
7. Crea el ID de cliente: **Clientes** (o **Credenciales → Crear credenciales → ID de cliente de OAuth**).
   - Tipo de aplicación: **Aplicación web**. Nombre: `Bitácora web`.
   - **Orígenes de JavaScript autorizados** → Añadir URI: `https://parias590.github.io` (sin barra final ni `/bitacora-cafe`).
   - Deja vacías las URI de redirección. Pulsa **Crear** y copia el **ID de cliente** (termina en `.apps.googleusercontent.com`).
8. En GitHub, abre `config.js`, pulsa el lápiz (Edit), pega el ID entre las comillas de `googleClientId` y pulsa **Commit changes**:
   ```js
   window.BITACORA_CONFIG = {
     googleClientId: "123456789012-abc123.apps.googleusercontent.com"
   };
   ```

### C. Usarla

9. Abre https://parias590.github.io/bitacora-cafe/ en el móvil y toca **Conectar Drive** (arriba a la derecha).
   Google dirá que *la app no está verificada*: es normal porque es tuya. Pulsa **Continuar** y permite el acceso.
10. Instálala en la pantalla de inicio:
    - **iPhone (Safari):** botón Compartir → **Añadir a pantalla de inicio**.
    - **Android (Chrome):** menú ⋮ → **Instalar aplicación**.
11. Repite el paso 9 en el resto de dispositivos con la misma cuenta de Google.

### Traer lo que ya tienes en la versión de Claude

En la versión de Claude, ve a **Historial → Exportar CSV**. Después, en esta app: **⚙ Ajustes → Importar…** y elige ese CSV.
Las preparaciones se combinan sin duplicarse (puedes importar el mismo archivo varias veces).

## Cómo funciona la sincronización

- Todo se guarda primero en el móvil, así que funciona sin conexión.
- Al guardar o borrar una preparación y al volver a abrir la app, se sincroniza con `bitacora-cafe.json` en tu Drive.
  Si dos dispositivos cambian cosas, se combinan: gana la versión más reciente de cada preparación.
- La app solo tiene permiso `drive.file`: ve únicamente los archivos que ella misma ha creado, nunca el resto de tu Drive.
- Google concede el acceso por 1 hora. Pasado ese tiempo, el botón de arriba cambia a **Sincronizar**; un toque y sube lo pendiente
  (normalmente sin volver a pedirte nada).
- No borres ni muevas a la papelera `bitacora-cafe.json`: es tu historial. Puedes renombrarlo o moverlo de carpeta.

## Actualizar la app

Sustituye `index.html` en GitHub y, en `sw.js`, sube el número de `CACHE` (por ejemplo `bitacora-cafe-v2`)
para que los móviles descarguen la versión nueva.
