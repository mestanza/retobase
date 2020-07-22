# Se descarga la imagen ubuntu 20.04
FROM ubuntu:20.04

# Desabilita el Prompt durante la instalacion
ARG DEBIAN_FRONTEND=noninteractive

# actualiza ubuntu
RUN apt update

# INstala nginx, php-fpm y supervisord desde el repositorio de ubuntu
RUN apt install -y nginx php-fpm supervisor composer curl && \
    rm -rf /var/lib/apt/lists/* && \
    apt clean
    
# define las variables de entorno (ENV)
ENV php_conf /etc/php/7.4/fpm/php.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf

# Habilita PHP-fpm en nginx 
COPY conf/default /etc/nginx/sites-available/default
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf}
    
# Se copia la configuracion de supervisor a la variable de entorno
COPY conf/supervisord.conf ${supervisor_conf}

#crea carpeta /run/php y le asigno un owner www-data a dicha carpeta y a /var/www/html
RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php

#copia datos de retobase-php a la carpeta de localhost de la imagen
COPY retobase-php /var/www/html
#ejecuta el localhost e instala composer para poder ejecutar el slim
RUN cd /var/www/html && composer install

# Configuracion de los Volume
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]

# Copia tarts.sh en la imagen y define los comandos por defecto en el contenedor
COPY start.sh /start.sh
CMD ["./start.sh"]

# asigna los puertos indicados para la aplicacion
EXPOSE 80 443

# Configura healthcheck para validad que todo este corriendo
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/