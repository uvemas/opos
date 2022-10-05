# Tema 31 Control de versiones. Subversion. GIT.

[gitkraken](https://www.gitkraken.com/learn/git)
## GIT. Conceptos básicos

[Tutoriales](https://linuxize.com/tags/git/)

El ciclo de trabajo básico en git es:

- editar ficheros
- añadir los ficheros modificados a la staging area
- hacer commit del contenido de la staging area i.e., crear un nuevo snapshot


Un **snapshot** es una copia del repositorio en un momento dado. En Pro Git pág. 82 se explica en detalle como se almacena en el 
snapshot cada directorio y fichero del repositorio.

La **staging area** es donde se almacenan los ficheros cuando estan listos para añadirse a un nuevo snapshot.

Un **commit** es un objeto que apunta a un snapshot del repositorio y a cada uno de los commits padre. El commit contiene metadatos 
como autor, commiter, email, mensaje... El commit, como todo en git, tiene un checksum para garantizar su integridad.

**Hacer commit** es añadir el contenido de la staging area al repositorio en un nuevo commit.

Una **rama** es un puntero movible que apunta *al último commit* de una línea de desarrollo.

A branch represents an independent line of development. Branches serve as an abstraction for the edit/stage/commit process. You can 
think of them as *a way to request a brand new working directory, staging area, and project history*. New commits are recorded in the 
history for the current branch, which results in a fork in the history of the project.

**HEAD** es un puntero que apunta a la rama en uso.

Hacer un **checkout** es moverse de un commit a otro en el árbol de commits. Como efecto colateral se actualiza el estado de nuestro 
directorio de trabajo al que tenía en el nuevo commit. La información va desde el repositorio al directorio de trabajo.

Cuando intercambiamos datos con un repositorio remoto:

- hacer push: añadir el commit local al repositorio remoto
- hacer fetch: descargar commits desde el repositorio remoto
- hacer pull: hacer fecth + merge

## Remotos

[Remotos](https://linuxize.com/post/how-to-remove-git-remotes/)

Git remote is a pointer that refers to another copy of the repository that is usually hosted on a remote server.

Every remote has an entry in `.git/config` file, for example:

    [core]
            repositoryformatversion = 0
            filemode = true
            bare = false
            logallrefupdates = true
    [remote "origin"]
            url = https://github.com/leo-editor/leo-editor
            fetch = +refs/heads/*:refs/remotes/origin/*
    [branch "master"]
            remote = origin
            merge = refs/heads/master
    [branch "devel"]
            remote = origin
            merge = refs/heads/devel


To add a remote use the command:

~~~ bash
$ git remote add short-name url
~~~

To list remotes use the command:

~~~ bash
$ git remote -v
origin	ssh://git@github.com/uvemas/ViTables.git (fetch)
origin	ssh://git@github.com/uvemas/ViTables.git (push)
~~~

To remove a remote use the command:

~~~ bash
$ git remote rm short-name
~~~

What the `git remote rm` command does is to remove all references to the remote repository (removing the entries about the remote 
repository from the `.git/config` file). *It does not remove the repository from the remote server*.

To fetch the whole remote repository:

    vicent@Perseus ➤ ~/leo-editor (master) 
    $ git fetch --all 
    Extrayendo origin
    remote: Enumerating objects: 6051, done.
    remote: Counting objects: 100% (5731/5731), done.
    remote: Compressing objects: 100% (1154/1154), done.
    remote: Total 5538 (delta 4701), reused 5204 (delta 4384), pack-reused 0
    Recibiendo objetos: 100% (5538/5538), 994.91 KiB | 646.00 KiB/s, listo.
    Resolviendo deltas: 100% (4701/4701), completado con 159 objetos locales.
    Desde https://github.com/leo-editor/leo-editor
      884b4bca3..5ad7423e2  master      -> origin/master
    * [nueva rama]          6.6.4       -> origin/6.6.4
      c8ff87e51..ee41a5dcb  devel       -> origin/devel
    * [nueva rama]          ekr-clean4  -> origin/ekr-clean4
    * [nueva rama]          ekr-typing2 -> origin/ekr-typing2
    * [nuevo tag]           v6.6.4      -> v6.6.4
    * [nuevo tag]           v6.6.5-devel -> v6.6.5-devel

While the git fetch command will fetch down all the changes on the server
that you don't have yet, it will not modify your working directory at all. It will
simply get the data for you and let you merge it yourself.

Por tanto, no se han añadidos ramas locales nuevas:

    $ git branch -v
      devel  c50915380 [detrás 1829] Merge pull request #2478 from tbpassin/devel-clean
    * master 5ad7423e2 [detrás 655] Correct link

Ni ha cambiado el estado de las ramas existentes:

    $ git status
    En la rama master
    Tu rama está detrás de 'origin/master' por 655 commits, y puede ser avanzada rápido.
      (usa "git pull" para actualizar tu rama local)

    nada para hacer commit, el árbol de trabajo está limpio

Si queremos actualizar la rama en la que estamos tenemos que mezclarla explícitamente:

    $ git pull
    Actualizando 884b4bca3..5ad7423e2
    Fast-forward
    .mypy.ini                                      |   72 +-
    PKG-INFO.TXT                                   |    2 +-
    README.md                                      |   11 +-
    leo/commands/abbrevCommands.py                 |    6 +-
    leo/commands/bufferCommands.py                 |    6 +-
    leo/commands/checkerCommands.py                |   10 +-
    leo/commands/commanderEditCommands.py          |    3 +-
    leo/commands/commanderFileCommands.py          |    7 +-
    leo/commands/commanderOutlineCommands.py       |   29 +-
    leo/commands/controlCommands.py                |    6 +-
    leo/commands/convertCommands.py                | 1058 ++++++++++++-------------
    leo/commands/editCommands.py                   |   26 +-
    ...

    $ git status
    En la rama master
    Tu rama está actualizada con 'origin/master'.

Y si queremos añadir ramas también tenemos que hacerlo explícitamente:

    $ git checkout 6.6.4
    Rama '6.6.4' configurada para hacer seguimiento a la rama remota '6.6.4' de 'origin'.
    Cambiado a nueva rama '6.6.4'

## Fusión de ramas

(rebase)[https://www.atlassian.com/es/git/tutorials/rewriting-history/git-rebase]
(rebase_grafico)[https://www.gitkraken.com/learn/git/git-rebase]

La fusión de ramas puede hacerse por dos mecanismos:

- merge
- rebase

`merge` es más intuitivo. `rebase` mantiene el historial más fácil de leer porque reemplaza bifurcaciones con cambios lineales.

The easiest way to integrate two branches, as we've already covered, is the merge command. It performs a three-way merge *between the 
two latest branch snapshots (C3 and C4) and the most recent common ancestor of the two (C2)*, creating a new snapshot (and commit).

*Rebasing replays changes from one line of work onto another in the order they were introduced, whereas merging takes the endpoints and 
merges them together.*

Note that the snapshot pointed to by the final commit you end up with, whether it's the last of the rebased commits for a rebase or the 
final merge commit after a merge, is the same snapshot - it's only the history that is different. Rebasing replays changes from one 
line of work onto another in the order they were introduced, whereas merging takes the endpoints and merges them together.
## Tags

Un tag es una etiqueta que se coloca en un determinado punto de la historia del repositorio (en un determinado commit). Su uso más
habitual es marcar los commit en los que se ha hecho una nueva release del software.

Hay dos tipos de tags:

- ligeros: son simplemente una referencia a un commit específico
- anotados: contienen información adicional (mensaje, tagger name, tagger email, fecha... y tienen checksum)

Si acabamos de hacer un commit  queremos ponerle un tag ligero:

    $ git tag v1.4

Si queremos ponerle un tag anotado:

    $ git tag -a v1.4 -m "my tagging message"

Si queremos poner un tag a un commit en concreto:

    $ git tag -a v1.4 commit_checksum

Si queremos listar los tags:

    $ git tag

Si queremos ver un tag en concreto:

    $ git show v1.4

## Stash y bisect

Para encontrar bugs que no sabemos cuando se han introducido es muy útil la herramienta `bisect`:

[bisect](https://apiumhub.com/es/tech-blog-barcelona/git-bisect/)
[bisect](https://picodotdev.github.io/blog-bitix/2022/04/como-usar-el-comando-git-bisect-para-descubrir-el-primer-commit-con-un-error/)

Otro comando muy utilizado es `stash`:

[stash](https://www.atlassian.com/es/git/tutorials/saving-changes/git-stash)
## Worktrees

[Worktrees](https://www.gitkraken.com/learn/git/git-worktree)

Un worktree permite trabajar simultáneamente con dos ramas.

En el flujo normal de trabajo trabajamos solo con una rama a la vez. Si queremos cambiar de rama tenemos que hacer commit en la rama
actual y luego hacer un checkout a la otra rama.

Pero puede pasar que tengamos que cambiar de rama antes de haber terminado nuestro trabajo en la rama actual, por tanto todavía no
estamos listos para hacer commit. En este caso tenemos dos opciones:

- hacer un stash de la rama actual y cuando volvamos a ella recuperar el stash
- crear un worktree donde trabajar con la otra rama

El worktree es mucho más cómodo. Nos permite trabajar con varias ramas simultáneamente. Cuando terminamos de trabajar en una rama
hacemos commit y borramos su worktree. Los cambios no se pierden porque se han aññadido al repo al hacer el commit.

