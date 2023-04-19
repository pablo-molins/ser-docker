# Montar un servidor nginx con Docker

## Probando docker

Como vamos a montar nuestra primer contenedor, si no lo hemos hecho ya, puede ser buena idea montar una imagen básica de nginx y probar cómo funciona.

Para ello, hacemos un comando muy sencillo:

> `docker run -p 80:80 nginx`

Docker automáticamente comprobará si tenemos una imagen llamada nginx en local y, si no es el caso, descargará dicha imagen desde **Docker Hub**, el repositorio oficial de imágenes. Las empresas pueden montar su propio Docker Hub privado, de tal manera que se distribuyan cómodamente imágenes dentro de una organización y no necesariamente con todo Internet.

Si todo ha ido bien, visitando [http://localhost](http://localhost) deberíamos ver la página de bienvenida por defecto de nginx.

Podemos parar nuestro contenedor de prubea. Si hacemos:

> `docker ps`

Veremos todos los contenedores que actualmente están funcionando. Copiamos el ID del contenedor que hemos creado antes y hacemos un:

> `docker stop ID`

Al volver a visitar [http://localhost](http://localhost) no aparecerá nada.

## Eligiendo método para poblar de contenido el servidor por defecto

Ya podemos tener un nginx funcionando, peor ahora necesitamos poder tener **nuestro** nginx funcioando, con todos los ficheros de configuración y todos los ficheros de contenido de la web colocados en su sitio.

Como podemos ver en este [tutorial oficial de nginx para Docker](https://www.nginx.com/blog/deploying-nginx-nginx-plus-docker/), hay dos maneras principales de lograr esto mismo:

1. Mantener los ficheros de configuración y el contenido en el ordenador host
2. Copiar los ficheros del host a dentro del contenedor.

¿Cuál debemos elegir? Si estamos montando nuestra página web, la segunda opción será muy engorrosa, pues cada vez que modifiquemos los ficheros, deberemos volver a construir la imagen (con `docker build`). Sin embargo, si queremos poder distribuir fácilmente nuestra propia página web, que ya está creada, esta última opción es la ideal, ya que simplifica el montar la web en cualquier parte con un solo `docker run`.

Como la web la reutilizamos de las prácticas anteriores, estamos en un caso similar a que la web ya esté montada y se prevean pocos casos (es como si estamos en un entorno de producción, no de desarrollo) y por lo tanto optaremos por el segundo método.

Para ello, crearemos nuestra propia imagen, con pequeñas modificaciones desde la imagen base de nginx.

## Creando nuestra imagen derivada

Para crear una imagen, debemos crear un fichero **Dockerfile**. Puedes encontrar el fichero que crearemos en este mismo directorio.

Debemos adaptarnos a algunas de las configuraciones por defecto de la imagen de nginx, teniendo cuidado con algunas de las rutas de los distintos ficheros.

Cuando la tenemos, ya podemos ejecutar el comando que creará la imagen:

> `docker build -t ser-nginx .`

## Ejecutar el contenedor

Crear el contenedor, teniendo la imagen, es muy sencillo:

> `docker run -p 80:80 ser-nginx`

Entrando en [http://localhost](http://localhost) deberíamos ver nuestra página.
