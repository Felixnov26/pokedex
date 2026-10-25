#  Proceso de Despliegue y Seguridad A+ - PokeDex Azure

Este documento detalla paso a paso el proceso de despliegue continuo (CI/CD) de la **PokeDex** en **Azure Static Web Apps** y la configuración de seguridad implementada para obtener la máxima calificación (**A+**) en auditorías de cabeceras de HTTP.

---

##  1. Configuración de Seguridad (`staticwebapp.config.json`)

Para garantizar la máxima protección contra vulnerabilidades (XSS, Clickjacking, MIME-sniffing) y asegurar la calificación **A+** en [securityheaders.com](https://securityheaders.com/), se creó el archivo `staticwebapp.config.json` en la raíz del proyecto.

### Contenido del Archivo:
{

"navigationFallback": {

"rewrite": "/index.html",

"exclude": ["/assets/", ".{js,css,png,jpg,ico,woff,woff2,ttf,eot,svg}"]

},

"globalHeaders": {

"Strict-Transport-Security": "max-age=63072000; includeSubDomains; preload",

"Content-Security-Policy": "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com data:; img-src 'self' data: https://assets.pokemon.com https://raw.githubusercontent.com; connect-src 'self' https://pokeapi.co https://beta.pokeapi.co; object-src 'none'; base-uri 'self'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests",

"X-Content-Type-Options": "nosniff",

"X-Frame-Options": "DENY",

"Referrer-Policy": "strict-origin-when-cross-origin",

"Permissions-Policy": "accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()",

"Cross-Origin-Opener-Policy": "same-origin",

"Cross-Origin-Resource-Policy": "same-origin"

}

}

### Explicación de Políticas Especiales: 
1. **`Strict-Transport-Security` (HSTS):** Fuerza conexiones HTTPS seguras con precarga por 1 año. 

2. **`X-Frame-Options: DENY`:** Evita que la app sea incrustada en `<iframe>` (protección contra Clickjacking). 

3. **`Content-Security-Policy` (CSP):** * `script-src 'self'`: Bloquea la ejecución de scripts no autorizados (Clave para calificación **A+**). * `style-src 'self' 'unsafe-inline'`: Permite los estilos dinámicos generados por Angular. * `img-src 'self' data: https: *`: Permite la carga de sprites desde la PokeAPI y GitHub sin romper las imágenes en el detalle de los Pokémon. * `connect-src 'self' https://pokeapi.co`: Autoriza las peticiones HTTP/REST hacia la PokeAPI.

 4. **`navigationFallback`:** Redirige las rutas internas de la SPA hacia `index.html` evitando errores 404 al recargar la página. 

---
 ##  2. Flujo de Despliegue Automático (CI/CD)
  El despliegue se realiza de forma automatizada mediante **GitHub Actions** al conectar Azure con el repositorio: 
  1. Al crear el recurso en Azure, se genera un archivo `.github/workflows/azure-static-web-apps-*.yml` automáticamente en el repositorio. 
  2. Cada `git push` a la rama principal dispara la compilación y despliegue del código. 
  ---
##  3. Comandos Ejecutados para el Despliegue  Paso a paso desde la terminal local:

bash

**1. Verificar estado de los archivos modificados**

git status

*** 2. Agregar los archivos de configuracion y documentacion***

git add .

**3. Crear el commit de despliegue**

git commit -m "feat: documentacion completa y configuracion de seguridad CSP para A+"

 **4. Sincronizar** 

git push origin main 

##  4. Verificación del Despliegue y Auditoría 

1. **URL de producción:** https://zealous-mushroom-006c6b80f.3.azurestaticapps.net/  Acceder al enlace proporcionado por Azure Static Web Apps. 

3. **Auditoría de Seguridad:** * Ingresar a https://securityheaders.com/. * 
Introducir la URL del proyecto desplegado en Azure. * **Resultado obtenido:** Calificación **A+**.
