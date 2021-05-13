# Iaw-Jekyll-Github-Pages
Creación de blogs con Jekyll y GitHub Pages

## Pasos a seguir

### Crea un repositorio de páginas estáticas de Github
Creamos un repositorio que se llama $user.github.io En mi caso, será japsasir.github.io y servirá para almacenar el contenido del blog.

### Prepara una máquina para Jekyll

Usaremos una máquina de Amazon con los correspondientes puertos abiertos en el grupo de seguridad:

- SSH 22
- HTTP 80
- 4000 (Acceso a web vía jekyll serve)

Instalamos Git (En Amazon AWS viene por defecto), Docker y Docker-Compose en la instancia:


```bash
apt update
apt-get install git
apt install docker docker-compose -y
systemctl enable docker
systemctl start docker
```

Clonamos el contenido del repositorio github.io en dicha máquina.

`sudo git clone https://github.com/japsasir/japsasir.github.io`

Le proporcionamos todos los permisos para que el usuario Docker pueda crear directorios dentro.

`chmod 777 japsasir.github.io`

Y nos dirigimos al directorio.

### Crea tu sitio

#### Jekyll new
Lanzamos el contenedor de Jekyll, que creará la estructura de directorios necesaria para crear nuestro blog.

`docker run -it --rm -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll new blog`


![Estructura de archivos](https://i.imgur.com/az4pAZ1.png)
La estructura creada.

Se nos creará un nuevo directorio llamado **blog**, debemos mover todo el contenido de ese directorio a la raíz del directorio clonado. Podemos hacerlo con un mv

`mv blog/* ./`



Podemos observar un archivo llamado **_config.yml** (Misma extensión que los que usamos con docker-compose) que nos permite alterar ciertas opciones: Título, correo, descripción...

![_config.yml](https://i.imgur.com/LiI3lVh.png)
Ejemplos de cambios de configuración. Para hacer pruebas, mejor dejar como theme 'mínima'

Para reaplicar la configuración de este archivo es necesario reiniciar el servicio (Ver jekyll serve más adelante)


#### Creando posts

En la carpeta **_posts** se almacenan todos los posts de nuestro sitio, donde vemos una archivo de ejemplo.

Los posts se escriben en lenguaje markdown (md), tienen una estructura que siempre se debe respetar y que podemos encontrar en ese post de ejemplo.

El título consta de:

- Año en el que se ha creado el post.
- Mes en el que se ha creado.
- Día del mes en el que se ha creado.
- Título del post.
Por ejemplo: 2021-05-13-welcome-to-jekyll.md

![](https://i.imgur.com/v5VRGGs.png)
Podemos copiar la cabecera incluida en este ejemplo para crear nuevos posts.

En este enlace podemos escribir con Markdown con facilidad, incluso sin haberlo visto antes:
https://markdown-editor.github.io/

Para actualizar el contenido del blog en nuestro repositorio (Recordemos, https://github.com/japsasir/japsasir.github.io) usaremos los comandos a los que estamos acostumbrados.

```bash
git add --all
git commit -m "Comentario descriptivo"
git push
```

#### Jekyll build
Este comando nos permite crear un sitio web a partir del contenido del proyecto Jekyll. Debe ejecutarse en el directorio donde tengas el contenido del blog y el resultado quedará en la carpeta _site.


#### Jekyll serve
Cuando tengamos nuestro sitio listo usaremos jekyll-serve para subir un sitio web estático, que se mostrará en el **puerto 4000** de nuestro localhost o la instancia de Amazon.
`
docker run -it --rm -p 4000:4000 -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll serve --force_polling`

Es el método más adecuado para hacer diferentes pruebas. Podemos detener el proceso haciendo **control+c** en el terminal. (Recordemos, necesario para recargar la configuración del archivo .yml)

Nota: La opción `--force_polling` actualiza el contenido del sitio de forma dinámica cuando realizas cambios en los archivos del proyecto.

## Referencias
- Guía original de Jose Juan Sánchez	https://josejuansanchez.org/iaw/practica-jekyll/index.html
- Playground de Jekyll en repositorio local	https://www.youtube.com/watch?v=ZHQ3IwIL590
