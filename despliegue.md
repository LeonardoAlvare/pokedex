# Guía de Despliegue: PokeDex App en Azure

Este documento describe el paso a paso para desplegar la aplicación Angular (PokeDex) en la nube utilizando **Azure App Services**.

## 1. Creación de la Web App en Azure

El primer paso consiste en aprovisionar la infraestructura en el portal de Azure. Crea un recurso de tipo Web App con la siguiente configuración:

- **Grupo de Recursos:** `rg-pokedex-prod`
- **Nombre de la Aplicación:** `app-pokedex-web-prod-la`
- **Runtime Stack:** `.NET 10 (LTS)` _(Se utiliza el entorno Windows/IIS para servir los archivos estáticos de Angular)._
- **Sistema Operativo:** Windows
- **Región:** Mexico Central
- **Plan de App Service:** Free F1

## 2. Preparación y Pruebas Locales

Antes de generar el compilado final, verifica que el proyecto funcione correctamente en el entorno local:

1. Descarga el código fuente del proyecto (o el archivo `.zip`) y descomprímelo.
2. Abre una terminal en la ruta del proyecto e instala las dependencias ejecutando:

   ```bash
   npm i
   ```

3. Levanta el servidor local para revisar el funcionamiento de la web:

   ```bash
   npm start
   ```

## 3. Configuración de Seguridad y Enrutamiento (web.config)

Dado que la aplicación se despliega en un entorno Windows (IIS) y es una Single-Page Application (SPA), es necesario configurar el servidor para manejar las rutas de Angular y agregar encabezados de seguridad.

1. Crea un archivo llamado `web.config` en el nivel raíz de la carpeta `src/`.
2. Agrega la ruta de este archivo dentro del arreglo de `assets` en el archivo `angular.json` para asegurar que Angular lo incluya en la carpeta final al momento de compilar.

## 4. Generación del Build (Compilación)

Una vez probada la aplicación y configurado el archivo `web.config`, procede a generar los archivos estáticos para producción. En la terminal, ejecuta el siguiente comando:

```bash
ng build
```

_Este comando genera una carpeta (generalmente llamada `dist/` o `dist/nombre-del-proyecto/`) con los archivos optimizados listos para subir al servidor._

## 5. Subida de Archivos mediante Kudu

Para finalizar el despliegue, sube los archivos directamente al servidor de Azure utilizando su herramienta de administración avanzada (Kudu):

1. En el portal de Azure, dentro de la Web App recién creada, accede a la sección de **Advanced Tools (Kudu)** y haz clic en "Go" o "Ir".
2. En la nueva pestaña, navega al directorio público del servidor usando la consola (CMD) o la interfaz gráfica: `D:\home\site\wwwroot`.
3. Elimina el archivo por defecto de Azure llamado `hostingstart.html`.
4. Arrastra y suelta todos los archivos generados dentro de tu carpeta de build local (los archivos, no la carpeta padre en sí) hacia el directorio `wwwroot` en Kudu.

Una vez finalizada la carga de archivos, la aplicación estará disponible y desplegada exitosamente.
