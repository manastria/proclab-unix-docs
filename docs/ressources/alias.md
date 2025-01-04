# Les alias pratiques pour la gestion des processus

## Introduction

Les **alias** (*shell aliases*) sont des raccourcis de commande essentiels pour gagner en efficacité lors de l'analyse et la gestion des processus sous Linux. Ce document vous explique comment configurer et utiliser les alias nécessaires aux activités pratiques sur les processus.

## Configuration des alias

### Emplacement des alias

Sous Linux, les alias permanents sont généralement stockés dans le fichier `~/.bash_aliases`. Ce fichier est automatiquement chargé par votre shell grâce à ces lignes présentes dans votre `~/.bashrc` :

```bash
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

### Alias essentiels pour la gestion des processus

Voici les alias que nous utiliserons dans nos activités. Ajoutez-les à votre fichier `~/.bash_aliases` :

```bash
# Affiche les processus avec les colonnes essentielles pour une analyse rapide :
#   - PPID : PID du processus parent.
#   - PID  : Identifiant du processus.
#   - STAT : État du processus (Running, Sleeping, etc.).
#   - TTY  : Terminal associé au processus (ou ? si aucun).
#   - USER : Utilisateur propriétaire du processus.
#   - CMD  : Commande utilisée pour démarrer le processus.
alias psess='ps -o ppid,pid,stat,tty,user,cmd'

# Affiche l'arborescence complète des processus à partir du shell courant.
# Options :
#   -p : Affiche les PIDs (identifiants des processus).
#   -c : Désactive le regroupement des processus similaires.
#   -l : Évite la troncature des lignes longues.
alias treeproc='pstree -p -c -l $$'

# Contraction de "Shell Tree", indiquant l'arborescence des processus autour du shell.
alias stree='pstree -p -c -l $$'

# Affiche tous les processus enfants de terminator
alias termtree='pstree -p -c -l $(ps -o ppid= -p $$)'
```

### Activation des alias

Après avoir ajouté ces alias, vous devez les activer avec la commande :

```bash
source ~/.bashrc
```

## Utilisation des alias

### psess : Affichage essentiel des processus

L'alias `psess` remplace la commande `ps` avec des options optimisées pour l'analyse des processus :

```bash
$ psess
  PPID   PID STAT TTY      USER     CMD
  1234  5678 S    pts/0    student  bash
  5678  5679 R    pts/0    student  procarbo
```

### treeproc : Visualisation de l'arborescence

```bash
$ treeproc
bash(12345)─┬─procarbo(12346)───procarbo(12347)
            └─pstree(12348)
```

La commande affiche une arborescence claire des processus avec leurs PID.

### stree et termtree : Analyse contextuelle

Ces alias permettent d'observer les processus dans leur contexte :
- `stree` : Focus sur les processus liés au shell actuel
- `termtree` : Vue des processus attachés à votre terminal

## Bonnes pratiques

1. **Documentation des alias**
   - Commentez chaque alias avec sa fonction et ses options
   - Groupez les alias par catégorie

2. **Maintenance**
   - Sauvegardez régulièrement votre fichier `~/.bash_aliases`
   - Supprimez les alias que vous n'utilisez plus

3. **Partage et réutilisation**
   - Partagez vos alias utiles avec vos collègues
   - Créez un dépôt Git pour vos dotfiles

## Points d'attention

- Un alias mal configuré peut masquer une commande système
- Les alias ne sont pas disponibles dans les scripts shell
- Les alias sont spécifiques à votre shell (bash dans notre cas)

## Exercices pratiques

1. Créez un alias `pstop` qui affiche les 5 processus consommant le plus de CPU :
```bash
alias pstop='ps aux --sort=-%cpu | head -6'
```

2. Créez un alias pour voir uniquement vos processus :
```bash
alias myproc='ps -u $USER'
```

## Pour aller plus loin

### Modèles d'alias avancés

```bash
# Alias avec paramètres via une fonction
psig() {
    ps -o pid,ppid,user,cmd -p $(pgrep -d, -f "$1")
}
```

### Ressources complémentaires

- Documentation Debian : [Shell Aliases](https://wiki.debian.org/ShellAlias)
- Communauté : [Awesome Bash Aliases](https://github.com/awesome-lists/awesome-bash)
- Fiche gclasse sur les alias : [Les alias sous Linux Debian](https://gclasse.fr/7fhk12nqs7)

## Conclusion

Les alias sont des outils puissants pour simplifier votre travail quotidien avec les processus sous Linux. Prenez le temps de les personnaliser selon vos besoins et de maintenir votre collection d'alias.
