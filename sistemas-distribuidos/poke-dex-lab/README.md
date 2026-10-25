# Pokédex Angular
Es una aplicación angular, creada por el desarrollador Keiler Mora, que simula un pokex y esta aplicación donde puede ser vista en vivo a traves del enlace https://keilermora.github.io/pokedex-angular/.
El proyecto fue desarrollado usando la librería de JavaScript [Angular](https://angular.io/) para crear la interfaz de usuario, en comunicación con la Api RESTful [PokéAPI](https://pokeapi.co/).


##  1. Creación de Cuenta y Configuración Inicial en Azure

### Paso 1: Registro en Microsoft Azure
1. Acceder al portal oficial de [Microsoft Azure](https://azure.microsoft.com/).
2. Crear una cuenta utilizando el correo institucional o personal.
3. Activar el crédito gratuito de suscripción (Azure for Students o Azure Free Tier).

### Paso 2: Creación del Recurso Azure Static Web Apps
1. En el buscador del portal de Azure, seleccionar **Static Web Apps**.
2. Hacer clic en **+ Crear**.
3. Configurar los datos básicos:
   * **Suscripción:** Suscripción activa de Azure.
   * **Grupo de recursos:** `rg-pokedex-prod` (o crear uno nuevo).
   * **Nombre del recurso:** `app-pokedex-portal-prod-fn`.
   * **Plan:** *Free* (Gratuito).
   * **Región:** *East US 2* (o la más cercana).
4. En la sección **Origen del despliegue**, seleccionar **GitHub** e iniciar sesión con la cuenta personal (`Felixnov26`).
5. Vincular el repositorio bifurcado (`Felixnov26/pokedex`) y la rama principal `main` / `master`.
6. En la sección **Detalles de compilación (Build Details)**:
   * **Preajustes de compilación:** *Angular*.
   * **Ubicación de la aplicación:** `/`.
   * **Ubicación de salida de la compilación:** `dist/pokedex` (o la carpeta de salida según `angular.json`).
7. Hacer clic en **Revisar y crear** y posteriormente en **Crear**.

---

##  2. Entorno Local y Prerrequisitos
* **Node.js:** Versión 18.x o superior.
* **Angular CLI:** `npm install -g @angular/cli`.
* **Git:** Para el control de versiones y sincronización con GitHub. (opcional)
* **Visual Studio Code:** Editor de código principal.

---

##  3. Clonación del Repositorio (Fork) y Dependencias


Dado que el proyecto fue obtenido mediante un **Fork** en GitHub, el flujo local es el siguiente:


## **1. Clonar el repositorio**
**```bash```**
 git clone https://github.com/Felixnov26/pokedex.git
 
 # Acceder al directorio 
 cd pokedex

 # Instalar dependencias
   npm install
   

## Ejecutar servidor local
     ng serve


## Referencias
[Keiler Mora](https://github.com/keilermora)