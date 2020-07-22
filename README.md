# retobase
Repositorio de la prueba del reto técnico IBM

## Detalles

Se tienen las carpetas ibm_nginx e ibm_backend, donde se tiene el contenido y los archivos de configuración para cada imagen
Así como tambien el archivo docker-compose.yml que genera ambos servicios.

Se usó el repo de PHP para habilitar el servicio de /sumar.


### Build

Al usar docker-compose, se ejecutaría el siguiente comando para levantar el contenedor con las imágenes del reverse-proxy y el backend

```
docker-compose up
```
Se ha incluido el HEALTHCHECK para el backend
Gracias nuevamente por la oportunidad.
