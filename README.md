# Dashma en Docker

Un Dashboard Minimalista inspirado en el concepto japonés "Ma" 🐳🏯

[![GitHub](https://img.shields.io/badge/GitHub-Repositorio-blue)](https://github.com/JLalib/dash-ma-docker)
[![Docker](https://img.shields.io/badge/Docker-Imagen-blue)](https://hub.docker.com/r/jlalib/dashma)
[![License](https://img.shields.io/badge/Licencia-MIT-green)](https://github.com/Lungshot/dashma/blob/main/LICENSE)

## 📋 Descripción general

Dashma es una aplicación "zen-inspired" que destaca por su ligereza. No consume apenas recursos de tu servidor y su carga es prácticamente instantánea. Al desplegarlo con Docker, garantizamos que nuestra "página de inicio" sea independiente y fácil de migrar o actualizar.

El objetivo de hoy es volver a lo esencial. En un mundo lleno de dashboards complejos como Dashy o Homepage, Dashma nace para ofrecer paz visual. Buscamos centralizar nuestros accesos directos en una interfaz que no solo sea funcional, sino que también sea un placer mirar.

## ✨ Características principales

- **Gestión de enlaces**: Permite gestionar una lista de enlaces con una tipografía cuidada y un diseño responsivo.
- **Negación del ruido**: Solo los enlaces que realmente usas, presentados de forma elegante.
- **Diseño responsivo**: Se adapta a diferentes tamaños de pantalla.
- **Configuración sencilla**: Archivos de configuración simples para definir tus enlaces.
- **Ligero y rápido**: Casi instantáneo en cargar, ideal para dispositivos con recursos limitados.
- **Fácil de personalizar**: Posibilidad de ajustar hoja de estilos y disposición de elementos para avanzados.
- **Docker oficial**: Imagen disponible en Docker Hub (jlalib/dashma:latest).

## 📋 Requisitos del sistema

- Docker y Docker Compose instalados
- Puerto 3000 disponible (o configurable)
- Espacio en disco para el archivo de configuración (`./config`)

## 🐳 Instalación

### Docker Compose (Recomendado)

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
services:
  dashma:
    container_name: dashma
    image: jlalib/dashma:latest # Apuntando a tu repositorio
    ports:
      - "3000:3000"
    volumes:
      - ./config:/app/config # Donde guardaremos nuestros enlaces
    restart: unless-stopped
```

Luego, inicia el servicio:

```bash
docker compose up -d
```

## ⚙️ Configuración

### Archivo de configuración

El contenedor espera un directorio `/app/config` montado desde el host. Dentro de ese directorio, puedes crear o modificar los archivos de configuración de Dashma (generalmente un archivo JSON o YAML que define tus enlaces y su apariencia).

Por defecto, el contenedor busque una configuración en `/app/config`. Puedes empezar con un archivo vacío y luego añadir tus enlaces mediante la interfaz web o editando directamente los archivos de configuración.

### Variables de entorno

La imagen no requiere variables de entorno para su funcionamiento básico. Sin embargo, revisa la documentación de la imagen para ver si hay opciones avanzadas (como cambiar el puerto interno, etc.).

### Puertos

- `3000`: Puerto por defecto para la interfaz web. Cambiar el mapeo si hay conflictos (ej: `"8080:3000"`).

## 🚀 Primeros pasos

1. **Crear el directorio y el archivo docker-compose.yml**
   ```bash
   mkdir dash-ma-docker && cd dash-ma-docker
   nano docker-compose.yml   # pega el contenido de arriba
   ```

2. **Crear el directorio de configuración**
   ```bash
   mkdir config
   ```

3. **Iniciar el contenedor**
   ```bash
   docker compose up -d
   ```

4. **Acceder a la interfaz web**
   Abre tu navegador y ve a `http://<ip-de-tu-servidor>:3000`.
   - Verás una pantalla limpia donde puedes comenzar a añadir tus enlaces favoritos.
   - La configuración se realiza de forma sencilla, permitiendo añadir servicios sin recargar la vista.

5. **Personalizar (opcional)**
   - Para los usuarios que quieran llevar el minimalismo al siguiente nivel, Dashma permite ajustes finos en su hoja de estilos y en la disposición de los elementos, manteniendo siempre la coherencia visual del concepto "Ma".
   - Consulta la documentación del proyecto original para más detalles sobre cómo personalizar la apariencia.

## 💡 Casos de uso

- **Página de inicio personal**: Centraliza tus enlaces más usados (servicios self-hosted, marcadores importantes, etc.) en una pantalla libre de distracciones.
- **Dashboard para equipos**: Comparte una instancia con tu equipo de trabajo o familiares para acceder rápidamente a herramientas comunes.
- **Entorno de pruebas**: Ideal para laboratorios o entornos de desarrollo donde se necesita una página de inicio ligera y personalizable.
- **Integración con otros servicios**: Combina con autenticación externa (como Authelia o OAuth2 Proxy) si deseas proteger el acceso.

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
Solo necesitas respaldar el directorio `config`:
```bash
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