# Installation de proclab 🚀

Pour réaliser cette activité pédagogique, vous aurez besoin d'installer deux éléments essentiels :

1. **Terminator** - Un terminal avancé permettant d'avoir plusieurs vues
2. **proclab** - Notre outil d'expérimentation

## 1. Installation de Terminator 🖥️

Commencez par ouvrir un terminal et installez Terminator avec la commande :

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

## 2. Installation de proclab 🛠️

!!! note "Pourquoi un téléchargement direct ?"
    Nous fournissons une version pré-compilée de **proclab** pour garantir une installation rapide et une expérience identique pour tous les étudiants.

### Étape 1 : Téléchargement

1. **[➡️ Téléchargez la dernière version de proclab](https://github.com/your-username/proclab-unix/releases/latest)**
2. Ouvrez un terminal dans le dossier contenant le fichier téléchargé
3. Rendez le fichier exécutable :

   ```bash
   chmod +x proclab
   ```

4. Déplacez le programme vers un répertoire système :

   ```bash
   mv proclab ~/bin/
   ```

### Étape 2 : Vérification

Pour vérifier l'installation, tapez dans votre terminal :

```bash
proclab --version
```

Vous devriez voir apparaître la version du programme.

## Test complet ✅

Réalisons un test rapide pour confirmer que tout fonctionne :

1. **Préparation** : 
    - Lancez Terminator
    - Divisez la fenêtre en deux avec ++ctrl+shift+e++

2. **Test** :
    - Terminal gauche : lancez `proclab`
    - Terminal droit : exécutez `ps -f`
    - ➡️ **proclab** devrait apparaître dans la liste des processus

!!! danger "Résolution des problèmes courants"
    === "Permission denied"
        1. Vérifiez que vous avez bien exécuté `chmod +x`
        2. Contrôlez la somme MD5 du fichier
    
    === "Command not found"
        1. Vérifiez que `/usr/local/bin` est dans votre `PATH`
        2. Essayez de relancer votre terminal

## Pour aller plus loin 🔍

Le code source de **proclab** est disponible sur GitHub. Si vous souhaitez explorer son fonctionnement interne ou le compiler vous-même :

!!! warning "Prérequis pour la compilation"
    - **Espace disque** : ~1.5 GB
    - **Dépendances** : Rust et ses outils
    - **Temps** : 10-15 minutes selon votre connexion

Les instructions détaillées sont disponibles dans le fichier `README.md` du dépôt.

---

## Prêt à commencer ? 🎯

Une fois **proclab** installé, vous pouvez débuter les activités. Direction le [premier scénario](activites/terminal.md) qui vous familiarisera avec les concepts de base !
