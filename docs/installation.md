# Installation de proclab 🚀

Pour réaliser ces activités pédagogiques, vous aurez besoin d'installer deux éléments essentiels :

1. **Terminator** - Un terminal avancé permettant d'avoir plusieurs vues
2. **Les outils proclab** - Notre suite d'outils d'expérimentation

## 1. Terminator 🖥️

!!! info "Déjà installé ?"
    Terminator est très probablement déjà installé sur votre système. Passez directement à la section suivante si c'est le cas !

Si Terminator n'est pas installé, ouvrez un terminal et exécutez :

```bash
sudo apt install terminator
```

Une fois installé, vous pouvez lancer Terminator de deux façons :

- Via le menu des applications
- En tapant `terminator` dans un terminal

!!! tip "Les raccourcis indispensables de Terminator"
    | Action | Raccourci |
    |--------|-----------|
    | Diviser verticalement | ++ctrl+shift+e++ |
    | Diviser horizontalement | ++ctrl+shift+o++ |
    | Fermer le terminal actif | ++ctrl+shift+w++ |
    | Naviguer entre les vues | ++ctrl+tab++ |

## 2. Installation des outils proclab 🛠️

!!! note "Pourquoi une archive précompilée ?"
    Nous fournissons une version pré-compilée de la suite **proclab** pour garantir une installation rapide et une expérience identique pour tous les étudiants.

### Étape 1 : Préparation du répertoire

!!! info "Configuration probablement déjà effectuée"
    Sur votre système, le répertoire `~/bin` est très certainement déjà créé et configuré.
    Vous pouvez le vérifier rapidement avec :
    ```bash
    echo $PATH | grep ~/bin
    ```
    Si vous voyez `~/bin` dans le résultat, tout est déjà configuré ! Passez directement à l'étape 2.

Si le répertoire n'est pas configuré (cas rare), voici la procédure :

1. Créez le répertoire `bin` :

   ```bash
   mkdir -p ~/bin
   ```

2. Ajoutez ces lignes à votre `~/.bashrc` :

   ```bash
   # Ajout du répertoire ~/bin au PATH
   if [ -d "$HOME/bin" ] ; then
       PATH="$HOME/bin:$PATH"
   fi
   ```

3. Appliquez les changements :

   ```bash
   source ~/.bashrc
   ```

### Étape 2 : Téléchargement et installation

1. **[➡️ Accédez à la page de la dernière version](https://github.com/manastria/proclab-unix/releases/latest)**
   - Cliquez sur "Assets" pour dérouler la liste des fichiers téléchargeables
   - Choisissez le fichier `proclab-X.Y.Z.tar.gz` (où X.Y.Z est le numéro de version)

!!! info "Qu'est-ce que les 'Assets' ?"
    Dans le contexte du développement logiciel et de GitHub, les "Assets" sont les fichiers joints à une version (*release*) d'un logiciel.
    Il peut s'agir des exécutables, des archives, de la documentation ou d'autres ressources nécessaires.
    C'est un terme technique couramment utilisé pour désigner les ressources ou éléments associés à un projet.

2. Décompressez l'archive dans votre répertoire `~/bin` :

   ```bash
   cd ~/bin
   tar xzf ~/Téléchargements/proclab-*.tar.gz
   ```

### Étape 3 : Vérification

Testez chaque outil pour vérifier l'installation :

```bash
procarbo --version     # Outil pour l'arborescence des processus
proclab --version     # Programme principal
procorhpan --version  # Outil pour les processus orphelins
procsignal --version  # Outil pour la gestion des signaux
procstate --version   # Outil pour l'état des processus
proczombi --version   # Outil pour les processus zombies
```

## Test complet ✅

Réalisons un test rapide pour confirmer que tout fonctionne :

1. **Préparation** :
    - Lancez Terminator
    - Divisez la fenêtre en deux avec ++ctrl+shift+e++

2. **Test** :
    Dans le terminal gauche, testez chaque outil :

    ```bash
    # Test de chaque outil
    procarbo         # Ctrl+C pour arrêter
    proclab          # Ctrl+C pour arrêter
    procorhpan       # Choisir 'Quitter' dans le menu
    procsignal       # Ctrl+C pour arrêter
    procstate        # Ctrl+C pour arrêter
    proczombi        # Choisir 'Quitter' dans le menu
    ```

!!! danger "Résolution des problèmes courants"
    === "Permission denied"
        1. Vérifiez les permissions des fichiers :
           ```bash
           chmod +x ~/bin/proc*
           ```
        2. Contrôlez que tous les fichiers sont bien extraits

    === "Command not found"
        1. Vérifiez que `~/bin` est dans votre PATH :
           ```bash
           echo $PATH | grep ~/bin
           ```
        2. Si non, relancez votre terminal après avoir modifié `.bashrc`

## Pour aller plus loin 🔍

Le code source de la suite **proclab** est disponible sur GitHub. Si vous souhaitez explorer son fonctionnement interne ou la compiler vous-même :

[➡️ Code source sur GitHub](https://github.com/manastria/proclab-unix)

!!! warning "Prérequis pour la compilation"
    - **Espace disque** : ~1.5 GB
    - **Dépendances** : gcc, make et autres outils de compilation
    - **Temps** : 5-10 minutes selon votre système

Les instructions détaillées sont disponibles dans le fichier `README.md` du dépôt.

---

## Prêt à commencer ? 🎯

Une fois la suite **proclab** installée, vous pouvez débuter les activités. Direction le [premier scénario](activites/terminal.md) qui vous familiarisera avec les concepts de base !
