# Tutorial git 01 - Repositorios remotos

Este es el segundo tutorial de la serie de capacitaciones de DH sobre git.

### Objetivos

- Clonar un repositorio git existente
- Realizar un aporte sobre el repositorio
- Publicar cambios a través de Push
- Realizar un pull request sobre Gitlab
- Aprender una noción básica sobre workflows en git
- Actualizar un repositorio cuando ocurrieron cambios en el origen

### Comandos aplicados

- [git clone](https://git-scm.com/docs/git-clone): Inicializa un repositorio clonándolo desde un remoto

### Documentación recomendada

- [Feature branching Workflow](http://nvie.com/posts/a-successful-git-branching-model/)

### Enlaces útiles

- [Meld - herramienta de merging](http://meldmerge.org/)
- [GitKraken](https://www.gitkraken.com/)

# Repositorios Remotos

Abramos una nueva consola bash sobre un nuevo directorio. Creemos un repositorio local git a partir de un repositorio existente en _gitlab_:

```sh
$ git clone git@gitlab.com:dh-external/git-workshop-exercise.git
Cloning into 'git-workshop-exercise'...
remote: Counting objects: 26, done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 26 (delta 5), reused 0 (delta 0)
Unpacking objects: 100% (26/26), done.
```

> **Cuidado:** es posible que _gitlab_ falle si no tenés las claves SSH cargadas. También se puede intentar el clone por HTTPS. Es posible que git solicite tu nombre de usuario y contraseña. Al final del tutorial hay información sobre los protocolos que soporta git y cómo utilizarlos.

El comando clone copia la base de datos de git de un repositorio remoto al local, copiando la base de datos entera. Esto significa que desde un primer momento, podrás acceder de forma local a toda la historia del repositorio.

```sh
$ cd tutorial-git-branching
$ git log --all --decorate --oneline --graph
* 9e78402 (origin/master-01-repos-remotos, master-01-repos-remotos) clone in branch
| * e87936c (HEAD -> master, origin/master) Add clone TOOD
|/  
* dc222ea Initial commit
```
> **TIP:** se pueden configurar alias a los comandos de git para invocarlos fácilmente. Por ejemplo, ejecutando `git config --global alias.logcheto "log --all --decorate --oneline --graph"` la próxima vez que tipees `git logcheto` se mostrará el log completo.

Por defecto, el repositorio se inicializa en el branch _master_ 

Para preparar el repo para este 

Al realizar trabajos en equipos sobre git es recomendable tener un proceso establecido de trabajo. Uno de los modelos más comunes es de realizar un branch por feature o fix (ver documentación recomendada). Vamos a simular un proceso corto de feature por branch.

El repositorio contiene un pequeño sitio con los comandos git por ahora aprendidos. Vamos a agregar uno nuevo al listado: `git reset`.

Antes de comenzar a trabajar, solo con el objetivo del presente workshop aca usuario debe crearse su branch master propio

```sh
$ git checkout -b git-stage-<username>
Switched to a new branch 'git-stage-<username>'

$ git push git-stage-<username>
Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: To create a merge request for git-stage-test01, visit:
remote:   https://gitlab.com/dh-external/git-workshop-exercise/-/merge_requests/new?merge_request%5Bsource_branch%5D=git-stage-test01
remote: 
To gitlab.com:dh-external/git-workshop-exercise.git
 * [new branch]      git-stage-test01 -> git-stage-test01

```

Luego creemos el nuevo branch con el feature

```sh
$ git checkout -b git-reset-<username>
Switched to a new branch 'git-reset-<username>'
```

Ahora investiguemos sobre `git reset` entrando a su documentación

```sh
$ git reset --help
```

> **TIP:** HEAD indica el commit en el que se encuentra la working copy

El comando `git reset` es útil para operar sobre los archivos que aún no están commiteados. Por ejemplo, si se desea descartar todos los cambios aplicados, es útil `git reset --hard`.

Agreguemos lo aprendido sobre el sitio, al final del listado. Una vez agregado podemos ver que el cambio se está por aplicar.

```sh
$ git status
On branch git-reset-<username>
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   sitio/index.html

no changes added to commit (use "git add" and/or "git commit -a")
$ git add sitio/index.html
$ git diff --staged
$ git commit -m "Agregado de reset"
[git-reset-<username> a20673a] Agregado de reset
 1 file changed, 4 insertions(+), 4 deletions(-)
$ git push origin git-reset-<username>
Counting objects: 4, done.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 352 bytes | 117.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
remote: 
remote: To create a merge request for git-reset-test01, visit:
remote:   https://gitlab.com/dh-external/git-workshop-exercise/-/merge_requests/new?merge_request%5Bsource_branch%5D=git-reset-test01
remote: 
To gitlab.com:dh-external/git-workshop-exercise.git
 * [new branch]      git-reset-test01 -> git-reset-test01
git-reset-<username>
```

Ya tenemos el nuevo feature en un branch propio, ahora tenemos que realizar un merge con _master_. Para esto vamos a crear un nuevo **pull request** o **merge request**. Estas son solicitudes de incorporación de cambios y sirven para que terceros revisen el código antes de realizar el merge. Todos los servicios sobre git ofrecen esta funcionalidad.

Entremos al [repositorio en Gitlab]() y creemos un nuevo _merge request_ hacia _el stage de cada uno_.




![Crear un Merge request]()

Antes de confirmar la creción de un nuevo _merge request_ se pueden revisar los cambios que incorpora el nuevo merge.


# Extra

### Protocolos de conexión a git

Git ofrece varios [protocolos de conexión](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols), como HTTPS y SSH. Ambos protocolos ofrecen transferencias seguras y eficientes.

#### SSH

Para configurar SSH en un repositorio no público es necesario generar una clave SSH en tu máquina (si no tenés una generada) y dar de alta la clave en el servidor git.

- [SSH en GitLab](https://gitlab.com/help/ssh/README)
- [SSH en Github](https://help.github.com/articles/connecting-to-github-with-ssh/)

#### HTTPS

Utilizar un repositorio desde HTTPS es sencillo ya que git soporta varios tipos de autenticación. Si el repositorio requiere autenticación, se pedirá usuario y clave cuando sea necesario. 
