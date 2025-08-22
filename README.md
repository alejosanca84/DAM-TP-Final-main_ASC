Web App Full Stack Base - Ionic frontend
========================================

El desarrollo realizado se baso en [Web App Full Stack Base](https://github.com/gotoiot/app-fullstack-base).

Para esta extensión se utilizo framework ionic para realizar el frontend.

### Instalación de las dependencias

Se requiere instalar `Docker` y `Docker Compose`. 

En [este artículo](https://www.gotoiot.com/pages/articles/docker_installation_linux/) publicado en nuestra web están los detalles para instalar Docker y Docker Compose en una máquina Linux.

Documentación adicional para instalación en otras plataformas o sistemas operativos:
-*[Docker](https://docs.docker.com/get-docker/)
-*[Docker Compose](https://docs.docker.com/compose/install/).

### Ejecución de la aplicación

Se requiere ejecutar el comando `docker compose up` desde la carpeta base del proyecto. El comando descargara las imágenes de TypeScript, Docker de node, de mysql-server y del mysql-admin de la DB, y arrancara. 

La Activación del cliente web se realiza ingresando a la URL [http://localhost:8100/](http://localhost:8100/) y para la DB ingresando a la [localhost:8001/](http://localhost:8001/). 

El garantizar el acceso las web, implica el adecuado funcionamiento del aplicativo.

# Nota: en caso de presentarse error durante el primer ejecución del aplicativo, debe detenerse utilizando Ctrl+C y volverse a correr el comando "docker compose up" (el backend espera que la DB esté creada al iniciar), puede que no se cree en la primera ejecución.

## Licencia
Este proyecto está bajo Licencia ([MIT](https://choosealicense.com/licenses/mit/)).

Archivo de licencia [LICENSE.md](LICENSE.md)

## Implementación 

### Web principal
En la web principal se observa la lista de sensores disponibles. Para mejorar la experiencia de usuario, cada elemento del listado cambia de color a rojo cuando el puntero del mouse se posiciona sobre ellos (Ver Figura 1).  

![Figura 1](/imagenes/Figura1.png)

### Dispositivo
Al dar click sobre uno de los sensores de la lista, se desplegara la web de ese dispositivo. En ella, se observa información sobre la última lectura de humedad y fecha y hora de registro.

Además, la interfaz presenta tres botones principales:

Control electroválvula: El botón cambia de color en función del estado  de la electroválvula.
Si está abierta, el botón será de color rojo y mostrará el texto "Cerrar EV".  
Si está cerrada, el botón será de color verde y mostrará el texto "Abrir EV".  
Mediciones: Permite acceder al listado de mediciones registradas por el sensor.  
Volver: Retorno a página principal con el listado de sensores.  
(Ver Figura 2)  

![Figura 2](/imagenes/Figura2.png)

### Lista de mediciones 
Al hacer click sobre el botón de mediciones, se despliega una web que presenta la lista detallada de los registros históricos de lecturas del sensor. Cada registro incluye la fecha, hora y valor de humedad, ordenados de la mas reciente hasta la más antigua.  

Bajo la lista tenemos el botón volver a la página del sensor correspondiente (Ver Figura 3).  

![Figura 3](/imagenes/Figura3.png)

### Servicios y endpoints 
Se creó un servicio (dispositivo.service.ts) que centraliza todas las peticiones hacia la base de datos. 

| **Método** | **Endpoint**                          | **Descripción**                                                                                  |
|------------|---------------------------------------|--------------------------------------------------------------------------------------------------|
| `GET`      | `/dispositivo`                       | Obtiene un listado de todos los dispositivos disponibles en la base de datos.                   |
| `GET`      | `/dispositivo/:id`                   | Recupera los datos de un dispositivo específico junto con su última medición registrada.        |
| `GET`      | `/dispositivo/:id/mediciones`        | Obtiene el historial completo de mediciones asociadas a un dispositivo, ordenadas por fecha.    |
| `POST`     | `/dispositivo/:id/valvula`           | Registra un nuevo estado para la electroválvula y una medición de humedad asociada al dispositivo. |
| `GET`      | `/dispositivo/:id/valvula/:id`       | Consulta el estado más reciente de apertura/cierre de la electroválvula asociada al dispositivo. |
