# Dashma Docker Setup

Un Dashboard Minimalista inspirado en el concepto japonés "Ma" 🐳🏯

[![GitHub](https://img.shields.io/badge/GitHub-Repositorio-blue)](https://github.com/JLalib/dash-ma-docker)
[![Docker](https://img.shields.io/badge/Docker-Imagen-blue)](https://hub.docker.com/r/jlalib/dashma)
[![License](https://img.shields.io/badge/Licencia-MIT-green)](https://github.com/Lungshot/dashma/blob/main/LICENSE)

## 📋 Descripción general

Dashma es una aplicación "zen-inspired" que destaca por su ligereza. No consume apenas recursos de tu servidor y su carga es prácticamente instantánea. Al desplegarlo con Docker, garantizamos que nuestra "página de inicio" sea independiente y fácil de migrar o actualizar.

El objetivo de hoy es volver a lo esencial. En un mundo lleno de dashboards complejos como Dashy o Homepage, Dashma nace para ofrecer paz visual. Buscamos centralizar nuestros accesos directos en una interfaz que no solo sea funcional, sino que también sea un placer mirar.

## ✨ Características principales

- **Ligero y rápido**: Casi sin consumo de recursos, carga instantánea.
- **Diseño minimalista**: Inspirado en el concepto japonés "Ma" (空間), el espacio negativo como elemento de diseño.
- **Gestión de enlaces**: Permite crear una lista de accesos directos a tus servicios favoritos.
- **Tipografía cuidada**: Texto legible y estéticamente agradable.
- **Diseño responsivo**: Se adapta a diferentes tamaños de pantalla.
- **Fácil de configurar**: Archivos de configuración sencillos en `/app/config`.
- **Contenedor Docker**: Aislamiento total y despliegue sencillo con `docker-compose up -d`.

## 📋 Requisitos del sistema

- Docker y Docker Compose instalados
- Puerto 3000 disponible (o configurable)
- Espacio en disco mínimo para la configuración

## 🐳 Instalación

### Docker Compose (Recomendado)

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
services:
  dashma:
    image: jlalib/dashma:latest
    container_name: dashma
    restart: unless-stopped
    # Required for ICMP ping monitoring (if using ping feature)
    cap_add:
      - NET_RAW
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - PORT=3000
      # COOKIE_SECURE: Set to 'true' only if NOT behind a reverse proxy (direct HTTPS)
      # Leave unset or 'false' when behind Caddy/nginx/Traefik that handles SSL
      # - COOKIE_SECURE=true
      # Optional: Set a custom session secret for enhanced security
      # In Portainer, add this as an environment variable:
      # SESSION_SECRET=your-secure-random-string-minimum-32-characters
      # - SESSION_SECRET=${SESSION_SECRET}
    volumes:
      - dashma_data:/app/src/data
      - dashma_uploads:/app/src/public/uploads

volumes:
  dashma_data:
  dashma_uploads:
```

Luego, inicia el servicio:

```bash
docker compose up -d
```

## ⚙️ Configuración

La configuración se realiza mediante archivos JSON o YAML en el directorio `./config` (mapeado a `/app/config` dentro del contenedor).

### Estructura de configuración

Crea un archivo `config.json` dentro de `./config` con el siguiente formato:

```json
{
  "title": "Mi Dashboard",
  "items": [
    {
      "title": "Plex",
      "url": "http://plex.local:32400/web",
      "icon": "plex"
    },
    {
      "title": "Portainer",
      "url": "http://portainer.local:9000",
      "icon": "docker"
    }
    // Añade más elementos según necesites
  ],
  "theme": {
    "background": "#ffffff",
    "text": "#333333",
    "accent": "#0066cc"
  }
}
```

Opciones disponibles para `icon`: Puedes usar nombres de Font Awesome 5 o dejar vacío para un icono genérico.

### Personalización avanzada

Para los usuarios que quieran llevar el minimalismo al siguiente nivel, Dashma permite ajustes finos en su hoja de estilos y en la disposición de los elementos, manteniendo siempre la coherencia visual del concepto "Ma".

## 🚀 Primeros pasos

1. **Crear el directorio y los archivos**
   ```bash
   mkdir dash-ma-docker && cd dash-ma-docker
   mkdir config
   nano docker-compose.yml   # pega el contenido de arriba
   nano config/config.json   # define tus enlaces y título
   ```

2. **Iniciar el contenedor**
   ```bash
   docker compose up -d
   ```

3. **Acceder a la interfaz**
   Abre tu navegador y ve a `http://<ip-de-tu-servidor>:3000`
   - Deberías ver una pantalla limpia con tus enlaces.
   - El espacio es el protagonista, siguiendo la filosofía "Ma".

4. **Ajustar y disfrutar**
   - Modifica `config/config.json` para añadir, eliminar o reordenar enlaces.
   - Los cambios se aplican en tiempo real (puede requerir recargar la página).
   - Experimenta con colores y temas para adaptarlo a tu gusto.

## 💡 Casos de uso

- **Página de inicio personal**: Reemplaza tu página de inicio del navegador con un dashboard personalizado.
- **Centro de control del Home Lab**: Acceso rápido a todos tus servicios autoalojados.
- **Compartir con familia o equipo**: Configura enlaces útiles para otros usuarios de tu red.
- **Minimalismo productivo**: Elimina el ruido visual y enfócate en lo esencial.

## 🔒 HTTPS con Caddy (acceso remoto seguro)

Si deseas acceder a Dashma desde fuera de tu red local de forma segura, puedes usar Caddy para obtener un certificado gratuito de Let's Encrypt.

### Configuración Caddyfile

```caddy
dashma.tudominio.com {
    reverse_proxy localhost:3000
}
```

> **Importante**: Dashma no tiene autenticación integrada. Si lo expones a Internet, considera añadir autenticación en Caddy o limitar el acceso por IP.

### Acceso remoto seguro

1. Configura Caddy con tu dominio y apunta a `localhost:3000`.
2. HTTPS se habilita automáticamente con Let's Encrypt.
3. Accede desde cualquier lugar: `https://dashma.tudominio.com`
4. ¡Disfruta de tu dashboard minimalista y seguro!

## 🛠️ Gestión y mantenimiento

### Ver registros
```bash
docker compose logs -f
```

### Actualizar a la última versión
```bash
docker compose pull
docker compose up -d
```

### Reiniciar el servicio
```bash
docker compose restart
```

### Ver estado de salud
```bash
docker compose ps
```

### Copia de seguridad de la configuración
```bash
# Solo necesitas respaldar el directorio de configuración
cp -r ./config ./config-backup-$(date +%Y%m%d)
```

### Restaurar desde copia de seguridad
```bash
docker compose stop
rm -rf ./config
cp -r ./config-backup-YYYYMMDD ./config
docker compose start
```

## 📝 Licencia

Este proyecto se basa en [Dashma](https://github.com/Lungshot/dashma) que está licenciado bajo la [MIT License](https://github.com/Lungshot/dashma/blob/main/LICENSE).

---

> ✨ **Nota**: Este repositorio contiene la configuración Docker y documentación extraída del tutorial de Genbyte: [Dashma en Docker: Un Dashboard Minimalista inspirado en el concepto japonés "Ma"](https://genbyte.blogspot.com/2026/03/dashma-en-docker-un-dashboard.html)