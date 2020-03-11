# Tutorial git 00 - Comandos básicos

Este es el primer tutorial de la serie de capacitaciones de DH sobre git.

### Objetivos

- Utilizar por primera vez git sobre consola
- Aprender a crear un nuevo repositorio
- Aplicar commits
- Visualizar el log de commits
- Crear branches
- Merge de branches

### Comandos aplicados

- [cd](http://linuxcommand.org/lc3_man_pages/cdh.html): cambia de directorio (Change Directory).
- [git add](https://git-scm.com/docs/git-add): agrega archivos al staging area
- [git branch](https://git-scm.com/docs/git-branch): crea un nuevo branch basado en el HEAD
- [git clone](https://git-scm.com/docs/git-clone): descarga un repositorio git
- [git commit](https://git-scm.com/docs/git-commit): versiona los archivos del staging area.
- [git diff](https://git-scm.com/docs/git-diff): visualiza diferencias entre dos áreas de git
- [git init](https://git-scm.com/docs/git-init): Inicializa un nuevo repositorio
- [git log](https://git-scm.com/docs/git-log): visualización de los últimos commits
- [git status](https://git-scm.com/docs/git-status): muestra el estado actual del working area
- [ls](https://linux.die.net/man/1/ls): lista los archivos y directorios.
- [touch](https://linux.die.net/man/1/touch): utilizado para crear un nuevo documento

### Alternativas interactivas

Este tutorial está pensado para realizarse sobre la consola de git, utilizando la herramienta real desde un principio. Si no estás muy acostumbrado a utilizar la consola de comandos o sentís mayor comodidad con tutoriales interactivos, podés realizar algunos de estos tutoriales básicos sobre git:

- [Tutorial Branching](https://learngitbranching.js.org/)
- [Tutoriales](https://try.github.io/)

### Documentación recomendada

- [Documentación de git oficial](https://git-scm.com/docs)
- [Tutoriales de Atlassian](https://www.atlassian.com/git/tutorials)
- [Cheatsheet de Github](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
- [Cheatsheet interactiva](http://ndpsoftware.com/git-cheatsheet.html)

# Git básico

## 0 - Configuración de git

Abramos una nueva consola bash sobre un nuevo directorio. Antes de comenzar chequeemos la versión

```sh
$ git --version
git version 2.17.1
```

Git requiere de datos del usuario para poder realizar commits. Si estás trabajando sobre tu computadora, podés configurar estos datos de forma global.

```sh
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```

> **TIP:** Si no es tu computadora, después configuraremos estos datos dentro del repositorio.

## 1 - Iniciando el proyecto

Existen dos formas de crear un repositorio de forma local: creando un nuevo repositorio con `git init`, o descargando un repositorio ya existente con `git clone`.

Sobre la consola creemos un nuevo repositorio.

```sh
$ git init dh-git-1
Initialized empty Git repository in /home/lferrigno/git/dh-git-1/.git/
```

El comando crea un nuevo directorio con el nombre puesto por parámetro. Si accedemos al nuevo directorio y visualizamos lo creado

```sh
$ cd dh-git-1
$ ls -a
.    ..   .git
```

Git genera una carpeta oculta `.git` con toda la información del repositorio. En tutoriales avanzados veremos qué contiene, pero por el momento no deberemos modificar ningún contenido dentro de ese directorio. 

## 2 - Primer commit

Creemos un nuevo documento

```sh
$ touch README.md
```

Veamos el estado actual del repositorio

```sh
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

El comando `git status` no solo muestra el estado, sino algunas recomendaciones para realizar. Es recomendable utilizarlo siempre antes de realizar alguna acción. Por primera vez, analicemos la información.


```sh
On branch master
```

La primera linea nos indica que estamos en el branch _master_. El branch master es similar a _trunk_ de _SVN_.

```sh
Initial commit
```

o

```sh
No commits yet
```

Indica que todavía el repositorio está vacío.

```sh
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

Lista los archivos que aún no están siendo versionados por git. 

Agreguemos el documento al staging area

```sh
$ git add README.md
```

y si revisamos el estado

```sh
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.md
```

Ahora ya está preparado el archivo para ser committeado.

> **TIP:** Si todavía no configuraste los datos de usuario, este es el momento de hacerlo en el repositorio. Si utilizás `git config user.email` y `git config user.name` se configurarán sólo para el proyecto en el que se está trabajando.

```sh
$ git commit -m "Agregar README"
[master (root-commit) b437453] Agregar README
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

> **TIP:** Cada commit tiene un id obtenido por una función de hash SHA-1. Cada referencia a un commit se puede realizar por el SHA-1 completo o una parte de éste.

Revisemos el estado

```sh
$ git status
On branch master
nothing to commit, working directory clean
```

y el log de commits

```sh
$ git log
commit b4374535c1857d3ab3e11ee87a205d5711b38250
Author: Leandro Ferrigno <lferrigno@digitalhouse.com>
Date:   Sun Mar 8 10:19:24 2020 -0300

    Agregar README
(END)
```

## 3 - Visualizando diferencias

Realicemos cambios sobre el archivo _README.md_. Luego visualicemos el estado

```sh
$ echo "# Tutorial git" >> README.md
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Git identifica el cambio en el _README.md_. Antes de realizar un commit, podemos visualizar por consola los cambios que se realizaron

```sh
$ git diff
diff --git a/README.md b/README.md
index e69de29..ad48a90 100644
--- a/README.md
+++ b/README.md
@@ -0,0 +1 @@
+# Tutorial git
```

Ahora realicemos el commit

```sh
$ git add .
$ git commit -m "Agregado de título en README"
[master 05ec311] Agregado de título en README
 1 file changed, 1 insertion(+)
$ git log
commit 05ec31136dccf79d75029283bf0aa9fb0418957c
Author: Leandro Ferrigno <lferrigno@digitalhouse.com>
Date:   Sun Mar 8 10:35:27 2020 -0300

    Agregado de título en README

commit b4374535c1857d3ab3e11ee87a205d5711b38250
Author: Leandro Ferrigno <lferrigno@digitalhouse.com>
Date:   Sun Mar 8 10:19:24 2020 -0300

    Agregar README
(END)
```

> **TIP:** El comando `git add .` agrega todos los archivos al staging area. 

## 4 - Trabajando con branches

Técnicamente, un branch es una referencia a un commit. Esto significa que la creación de un branch implica solo la creación de una entidad cuya referencia es un commit específico.

Actualmente sólo existe el branch _master_. Podemos ver dónde referencia

```sh
git show-branch --sha1-name master
[05ec311] Agregado de título en README
```

> **TIP:** El manual de cada comando de git se visualiza tipeando `git <comando> --help`.

Creemos un nuevo branch

```sh
$ git branch primer-branch
```

Ahora visualicemos los branches creados

```sh
$ git branch
* master
  primer-branch
```

> **TIP:** El asterisco indica sobre qué branch se encuentra la working copy.

```sh
$ git checkout primer-branch
Switched to branch 'primer-branch'
```

> **TIP:** el comando `git checkout -b <branch>` aplica los comandos `git branch <branch>` y `git checkout <branch>`. Git tiene muchas abreviaciones o shorthands 

De esta forma tenemos un nuevo branch generado a partir de _master_. Ambos comparten la misma hitoria, pero nuevos commits no impactarán en master. Los branches tienen la utilidad de poder diverger en el trabajo. Por ejemplo, se podrá crear un nuevo branch por funcionalidad, la cual una vez terminada se unirá a _master_.

Creemos un nuevo documento y realicemos el commit

```sh
$ echo "Nuevo documento" >output.txt
$ git add .
$ git commit -m "Commit sobre mi primer branch"
[primer-branch 5f3bdfc] Commit sobre mi primer branch
 1 file changed, 1 insertion(+)
 create mode 100644 output.txt
```

Veamos la historia de todos los branches

```sh
$ git log --all --decorate --oneline --graph
* 5f3bdfc (HEAD -> primer-branch) Commit sobre mi primer branch
* 05ec311 (master) Agregado de título en README
* b437453 Agregar README
```

> **TIP:** HEAD indica el commit en el que se encuentra la working copy

El branch _master_ está un commit atrasado con respecto al primer branch. Si queremos impactar los cambios de _primer-branch_ en _master_, primero tenemos que realizar un switch a _master_.

```sh
$ git checkout master
Switched to branch 'master'
```

Revisemos que _master_ realmente no tenga el último commit.

```sh
$ git log
commit 05ec31136dccf79d75029283bf0aa9fb0418957c
Author: Leandro Ferrigno <lferrigno@digitalhouse.com>
Date:   Sun Mar 8 10:35:27 2020 -0300

    Agregado de título en README

commit b4374535c1857d3ab3e11ee87a205d5711b38250
Author: Leandro Ferrigno <lferrigno@digitalhouse.com>
Date:   Sun Mar 8 10:19:24 2020 -0300

    Agregar README
(END)
```

La unión entre dos branches se llama **merge**. Realicemos el merge _primer-branch_ sobre _master_.

```sh
$ git merge primer-branch
Updating 05ec311..5f3bdfc
Fast-forward
 output.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 output.txt
```

> **TIP:** Existen distintas formas de realizar un merge. Algunos generan un commit, otros como _Fast-forward_ no. Más adelante veremos cuándo se aplica cada uno y sus diferencias.


# Ejercicios

1. ¿Qué pasa si al realizar un merge le agregamos la opción `--no-ff`? Para esto crear un nuevo branch, realizar un commit y volver a mergear a master con esta opción. ¿Qué ocurre con el log?
2. ¿Qué utilidad tiene cada una de estas visualizaciones sobre el log?
  - `git log`
  - `git log --all --decorate --oneline --graph`
  - `git log --graph --abbrev-commit --decorate --date=relative --all`
  - `git log --all --graph --decorate=short --color`
  - [Visualizadores con interfaz gráfica.](https://git-scm.com/downloads/guis/)
3. ¿Se puede crear un branch a partir de un commit? ¿Cuál es el comando?
