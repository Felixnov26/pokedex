#  Proceso de Despliegue - PokeDex 

Este archivo detalla cada uno de los pasos realizados para desplegar la aplicación **PokeDex** en la nube utilizando **Azure Static Web Apps** y la sincronización con **GitHub Actions**.

##  Paso 1: Configuración previa en GitHub y Fork

1.  Se realizó un **Fork** del repositorio oficial de la PokeDex hacia tu cuenta personal de GitHub (`Felixnov26/pokedex`) para tener el control absoluto del código fuente y la rama principal (`master`).
    
2.  Se clonó el repositorio en el entorno local utilizando la terminal integrada de **Visual Studio Code** para realizar las configuraciones pertinentes.
    

##  Paso 2: Creación del Recurso en el Portal de Azure

1.  **Acceso a Azure:** Ingresar al portal oficial de [Microsoft Azure](https://azure.microsoft.com/) con las credenciales correspondientes.
    
2.  **Selección del Servicio:** En la barra de búsqueda superior, buscar y seleccionar **Static Web Apps**.
    
3.  **Configuración del Recurso:** Hacer clic en el botón **+ Crear** y rellenar los parámetros básicos del asistente:
    
    -   **Suscripción:** Seleccionar la suscripción activa de Azure.
        
    -   **Grupo de recursos:** Crear o seleccionar un grupo existente (por ejemplo,  `rg-pokedex-prod`).
        
    -   **Nombre del recurso:** Asignar un nombre identificativo como `app-pokedex-portal-prod-fn`.
        
    -   **Plan de hospedaje:** Seleccionar el plan **Free** (Gratuito).
        
    -   **Región:** Elegir la región más cercana (ej.  _East US 2_).
        
4.  **Vinculación con GitHub:**
    
    -   En la sección de _Origen del despliegue_, hacer clic en el botón de **Iniciar sesión con GitHub**.
        
    -   Autorizar a Azure para acceder a la cuenta (`Felixnov26`).
        
    -   Seleccionar la organización, el repositorio bifurcado (`pokedex`) y la rama correspondiente (`master`).
        

##  Paso 3: Configuración de los Detalles de Compilación (Build Details)

Durante el asistente de creación en Azure, se establecieron las rutas de compilación para que el sistema reconozca automáticamente que se trata de una aplicación desarrollada en **Angular**:

-   **Preajustes de compilación (Build Presets):**  _Angular_.
    
-   **Ubicación de la aplicación (App location):**  `/` (raíz del proyecto).
    
-   **Ubicación de la API (Api location):**  _(Dejar en blanco si no se utiliza backend propio)_.
    
-   **Ubicación de salida (App artifact location):**  `dist/pokedex` (carpeta donde Angular genera los archivos compilados para producción).
    

Una vez revisados los parámetros, se hizo clic en **Revisar y crear** y finalmente en **Crear**.

##  Paso 4: Automatización del Despliegue (CI/CD con GitHub Actions)

1.  Al finalizar la creación en Azure, el servicio genera automáticamente un archivo de flujo de trabajo (_Workflow_) de **GitHub Actions** en el repositorio, ubicado en la ruta `.github/workflows/`.
    
2.  Este archivo YAML contiene las instrucciones para que cada vez que se suba código (_push_) a la rama principal, GitHub Actions compile de forma autónoma la aplicación Angular y la publique en Azure Static Web Apps.
    

##  Paso 5: Sincronización y Envío Final desde VS Code

Para asegurar que los cambios de documentación, configuración de seguridad (`staticwebapp.config.json`) y ajustes estén sincronizados en la nube, se ejecutaron los siguientes comandos en la terminal integrada de Visual Studio Code:
bash

**1. Verificar estado de los archivos modificados**

git status

 **2. Agregar los archivos de configuracion y documentacion**

git add .

**3. Crear el commit de despliegue**

git commit -m "feat: documentacion completa y configuracion de seguridad CSP para A+"

 **4. Sincronizar** 

git push origin main 

---

##   Configuración de Seguridad 

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

3. **`Content-Security-Policy` (CSP):** * `script-src 'self'`: Bloquea la ejecución de scripts no autorizados (Clave para calificación **A+**).
4. * `style-src 'self' 'unsafe-inline'`: Permite los estilos dinámicos generados por Angular. 
5. * `img-src 'self' data: https: *`: Permite la carga de sprites desde la PokeAPI y GitHub sin romper las imágenes en el detalle de los Pokémon. 
6. * `connect-src 'self' https://pokeapi.co`: Autoriza las peticiones HTTP/REST hacia la PokeAPI.
 7. **`navigationFallback`:** Redirige las rutas internas de la SPA hacia `index.html` evitando errores 404 al recargar la página. 

---

##  Verificación del Despliegue y Auditoría 

1. **URL de producción:** https://zealous-mushroom-006c6b80f.3.azurestaticapps.net/  Acceder al enlace proporcionado por Azure Static Web Apps. 

3. **Auditoría de Seguridad:** * Ingresar a https://securityheaders.com/. * 
Introducir la URL del proyecto desplegado en Azure. * **Resultado obtenido:** Calificación **A+**.
