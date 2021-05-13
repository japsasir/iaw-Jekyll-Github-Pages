# iaw-Jekyll-Github-Pages
Creación de blogs con Jekyll y GitHub Pages

## Playground de Jekyll

https://www.youtube.com/watch?v=ZHQ3IwIL590

## Pasos a seguir

### Crea un repositorio de páginas estáticas de Github
Creamos un repositorio que se llama $user.github.io En mi caso, será japsasir.github.io y servirá para almacenar el contenido del blog.

### Prepara una máquina para Jekyll

Usaremos una máquina de Amazon con los correspondientes puertos abiertos en el grupo de seguridad:

SSH 22
HTTP 80

Instalamos Git, Docker y Docker-Compose en la instancia:


apt update
apt-get install git
apt install docker docker-compose -y
systemctl enable docker
systemctl start docker

Clonamos el contenido del repositorio github.io en dicha máquina 
sudo git clone https://github.com/japsasir/japsasir.github.io

### Crea tu sitio

#### Jekyll new
Lanzamos el contenedor de Jekyll, que creará la estructura de directorios necesaria para crear nuestro blog.

docker run -it --rm -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll new blog

Se nos creará un nuevo directorio llamado blog, debemos mover todo el contenido de ese directorio a la raíz del directorio clonado. Podemos hacerlo con un mv

mv blog/* ./

#### Creando posts

En la carpeta _posts se almacenan todos los posts de nuestro sitio, donde vemos una archivo de ejemplo.

Los posts se escriben en lenguaje markdown, tienen una estructura que siempre se debe respetar y que podemos encontrar en ese post de ejemplo.

El título consta de:

    Año en el que se ha creado el post.
    Mes en el que se ha creado.
    Día del mes en el que se ha creado.
    Título del post

En este enlace podemos escribir con Markdown con facilidad, incluso sin haberlo visto antes:
https://markdown-editor.github.io/

Para actualizar el contenido del blog en nuestro repositorio (Recordemos, https://github.com/japsasir/japsasir.github.io) usaremos los comandos a los que estamos acostumbrados.

git add -all
git commit -m "Comentario descriptivo
git push

#### Jekyll build
Este comando nos permite crear un sitio web a partir del contenido del proyecto Jekyll. Debe ejecutarse en el directorio donde tengas el contenido del blog.



#### Jekyll serve
Cuando tengamos nuestro sitio listo usaremos jekyll-serve para subir un sitio web estático, que se mostrará en el puerto 4000 de nuestro localhost o la instancia de Amazon.

docker run -it --rm -p 4000:4000 -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll serve --force_polling

Nota: La opción "--force_polling" actualiza el contenido del sitio de forma dinámica cuando realizas cambios en los archivos del proyecto.