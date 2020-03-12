# Tutorial git 00 - Comandos básicos

Este es el primer tutorial de la serie de capacitaciones de DH sobre git.


### Objetivos

- Trabajar con brances y diferentes estrategias de mergeo
### Comandos aplicados


# Git branching

## 0 - Clonar repositorio


Abramos una nueva consola bash sobre un nuevo directorio. Creemos un repositorio local git a partir de un repositorio existente en _gitlab_:

```sh
$ git clone git@gitlab.com:dh-external/git-workshop-exercise.git
Cloning into 'git-workshop-exercise'...
remote: Counting objects: 26, done.
remote: Compressing objects: 100% (18/18), done.
remote: Total 26 (delta 5), reused 0 (delta 0)
Unpacking objects: 100% (26/26), done.
```

## 1 - Branch por usuario

Cada uno se va a crear un branch propio para trabajr como si fuese su propio _master_

> **TIP:** Si se hizo algun taller anterior puede que ese branch ya exista, puede usarse el mismo

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
## 2 - Nuevos Branches

Vamos a suponer que cada uno de nosotros son 2 devs que trabaja cada uno en un feature diferente.

Crear un branch para cada uno de esos features a partir del de stage

```sh
$ git checkout -b git-feature1-<username>
Switched to a new branch 'git-feature1-<username>'

$ git checkout -b git-feature2-<username>
Switched to a new branch 'git-feature2-<username>'
```


## 3 - Develop feature

Hacer ediciones en ambos branches que no generen conflicto

Agregarlos al repo y en cada uno hacer el merge request correspondiente, aprobandolo
## 4 - Analizar resultado

Ir al branch master y revisar el log

## 5 - Develop 2 nuevos features merge

Crear dos nuevos branches a partir de master

Sin generar conflicto:

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Luego para cada branch hacer el merge request y aprobarlo

Analizar resultados en master


## 6 - Develop 2 features con conflicto

Crear dos nuevos branches a partir de master

GENERANDO conflicto:

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Luego para cada branch hacer el merge request. En alguno de alguna manera resolver el conflicto.

Analizar resultados en master

## 7 - Develop  2 features rebase

Crear dos nuevos branches a partir de master

GENERANDO conflicto:

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Hacer una modificacion feature 1 y commit

Hacer una modificacion feature 2 y commit

Luego para cada branch hacer el merge request. En el segundo se va a tener que usar rebase

Analizar resultados en master


