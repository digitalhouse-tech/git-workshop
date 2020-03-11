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
$ git clone git@gitlab.cysonline.com.ar:CAPACITACIONES/tutorial-git-branching.git
Cloning into 'tutorial-git-branching'...
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
*   767bd9e (origin/master) Merge branch 'agregado-clone'
|\  
| * f696b73 Agregado de comando clone
|/  
* 8c9ec9c (HEAD -> master, origin/master-01-repos-remotos) Agregado script de set up del ejercicio 01
* 3640b58 Creado el sitio
```
> **TIP:** se pueden configurar alias a los comandos de git para invocarlos fácilmente. Por ejemplo, ejecutando `git config --global alias.logcheto "log --all --decorate --oneline --graph"` la próxima vez que tipees `git logcheto` se mostrará el log completo.

Por defecto, el repositorio se inicializa en el branch _master_ 

Para preparar el repo para este tutorial, es necesario ejecutar el script

```sh
$ ./scripts/01-repo-remoto.sh
Completado
```

Al realizar trabajos en equipos sobre git es recomendable tener un proceso establecido de trabajo. Uno de los modelos más comunes es de realizar un branch por feature o fix (ver documentación recomendada). Vamos a simular un proceso corto de feature por branch.

El repositorio contiene un pequeño sitio con los comandos git por ahora aprendidos. Vamos a agregar uno nuevo al listado: `git reset`.

Antes de comenzar a trabajar, creemos el nuevo branch con el feature

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

Agreguemos lo aprendido sobre el sitio, al final del listado. Una vez agregado podemos ver que el cambio se está por aplicar

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
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 407 bytes | 407.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
To https://gitlab.cysonline.com.ar/CAPACITACIONES/tutorial-git-branching.git
 * [new branch]      git-reset-<username> -> git-reset-<username>
```

Ya tenemos el nuevo feature en un branch propio, ahora tenemos que realizar un merge con _master_. Para esto vamos a crear un nuevo **pull request** o **merge request**. Estas son solicitudes de incorporación de cambios y sirven para que terceros revisen el código antes de realizar el merge. Todos los servicios sobre git ofrecen esta funcionalidad.

Entremos al [repositorio en Gitlab]() y creemos un nuevo _merge request_ hacia _master_.

![Crear un Merge request]()

Antes de confirmar la creción de un nuevo _merge request_ se pueden revisar los cambios que incorpora el nuevo merge.

![Diferencias branches]()

Al generarse el nuevo _merge request_ vemos que hay un conflicto que impide el merge automático.

![Conflicto]()

Esto se puede corregir por el usuario que controla el branch _master_ o desde el branch que se intenta mergear. Ésta última va a generar un commit más en la historia, pero facilita el trabajo del revisor de código.

Sobre la consola, mergeemos con el branch _master_

```sh
$ git fetch
$ git merge origin/master
Auto-merging sitio/index.html
CONFLICT (content): Merge conflict in sitio/index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Los conflictos son marcados por git dentro de los archivos mismos. Por eso, mientras se está mergeando es posible que proyectos no compilen o corran correctamente. si vemos el documento en conflicto, la parte del conflicto está denotada por menores (<), iguales (=) y mayores (>).

```html
<<<<<<< HEAD
                                <td>reset</td>
                                <td>Restaura HEAD a un estado específico</td>
=======
                                <td>checkout</td>
                                <td>Cambia de branch o restaura los archivos del working tree</td>
>>>>>>> origin/master
```

La primera parte del conflicto muestra lo que se encuentra en el _HEAD_. La segunda los cambios que se desean impactar por el merge. Al final muestran de qué branch o commit proviene el conflicto.

En **Enlaces útiles** se listan algunas herramientas que facilitan la resolución de conflictos. Al instalar alguna y configurarlo en git, se puede invocar desde la consola directamente la resolución de conflictos

```sh
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc codecompare emerge vimdiff
Merging:
sitio/index.html

Normal merge conflict for 'sitio/index.html':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff): 
```

En este caso, necesitamos resolver el conflicto preservando las dos partes, una después de la otra. 

```sh
$ git status
On branch git-reset-<username>
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   sitio/index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	sitio/index.html.orig
```

El estado de git aún marca que se está mergeando. Para concluir el merge es necesario commitear.

> **CUIDADO:** Algunas herramientas de merging dejan archivos _.orig_, estos no hay que commitearlos al repositorio.


```sh
$ git commit
```

Va a abrir **Vi**, con el mensaje automático de merging. Para aceptar los cambios se debe guardar y salir con `:wq`.

```sh
[git-reset-<username> d1aa21a] Merge remote-tracking branch 'origin/master' into git-reset-mcordischi
$ git log
commit d1aa21afa8c94fe16e6e3be359fc5de49604861d (HEAD -> git-reset-<username>)
Merge: a20673a 767bd9e
Author: Martín Cordischi <mcordischi@gmail.com>
Date:   Mon Nov 14 10:19:32 2017 -0300

    Merge remote-tracking branch 'origin/master' into git-reset-<username>
$ git push origin git-reset-<username>
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 474 bytes | 474.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
To https://gitlab.cysonline.com.ar/CAPACITACIONES/tutorial-git-branching.git
   a20673a..d1aa21a  git-reset-<username> -> git-reset-<username>
```

![Merge request sin conflictos](images/gitlab-mr-no-conflictos.png)

Ahora el feature está listo para mergearse a _master_.

> **NOTA:** Es posible que no puedas visualizar el mismo mensaje, ya que si el usuario de GitLab no tiene permisos para commitear en master, no se le va a habilitar el botón de merge.

# Ejercicios

1. Hay un archivo en la raíz del proyecto llamado _.gitignore_. 
    - ¿Qué utilidad tiene? 
    - ¿Qué tipo de archivos pondrías dentro de ese listado?
    - ¿Se debería agregar la linea _*.orig_ para evitar commitear los archivos generados por las _mergetools_?

> **TIP:** Github tiene publicado templates de gitignore para todos los lenguajes en [github/gitignore](https://github.com/github/gitignore)

2. ¿Existe alguna diferencia entre el branching de git y de SVN? Qué notaste?
3. ¿Qué pasa si en vez de aplicar el comando `git merge origin/master` se aplica el comando `git rebase origin/master`? ¿Hay alguna diferencia en el código final? ¿Y la historia?

> **TIP:** Se puede forzar a que un branch apunte a un commit específico con `git checkout -b <nombre- branch> <commit>`. De esta forma se puede generar un branch que apunte al commit previo al merge con master, y repetir los pasos pero con el comando `rebase`

# Extra

### Protocolos de conexión a git

Git ofrece varios [protocolos de conexión](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols), como HTTPS y SSH. Ambos protocolos ofrecen transferencias seguras y eficientes.

#### SSH

Para configurar SSH en un repositorio no público es necesario generar una clave SSH en tu máquina (si no tenés una generada) y dar de alta la clave en el servidor git.

- [SSH en GitLab](https://gitlab.com/help/ssh/README)
- [SSH en Github](https://help.github.com/articles/connecting-to-github-with-ssh/)

#### HTTPS

Utilizar un repositorio desde HTTPS es sencillo ya que git soporta varios tipos de autenticación. Si el repositorio requiere autenticación, se pedirá usuario y clave cuando sea necesario. 
