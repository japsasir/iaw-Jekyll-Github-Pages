# iaw-Jekyll-Github-Pages
Creación de blogs con Jekyll y GitHub Pages

## Playground de Jekyll

https://www.youtube.com/watch?v=ZHQ3IwIL590

## Pasos a seguir

### Crea un repositorio de páginas estáticas de Github
Creamos un repositorio que se llama $user.github.io. En mi caso, será japsasir.github.io y servirá para almacenar el contenido del blog.

### Prepara una máquina

Usaremos una máquina de Amazon con los correspondientes puertos abiertos:

SSH 22
HTTP 80

Clonamos el contenido del repositorio github.io en dicha máquina https://github.com/japsasir/japsasir.github.io

Instalaremos docker y lo inicializaremos. Descargamos un contenedor docker de Jekyll