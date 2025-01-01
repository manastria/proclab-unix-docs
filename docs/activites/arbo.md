# Scénario : Arborescence des processus

## Objectif pédagogique

Apprendre à observer, comprendre et analyser l’**arborescence des processus** sous Linux.  
Les étudiants utiliseront des commandes comme `ps`, `pstree` et `htop`, ainsi que des alias pratiques, pour visualiser les relations parent-enfant entre processus.

## 1. Introduction : Qu’est-ce qu’une arborescence de processus ?

### **Définition**  

Dans Linux, les processus sont organisés sous forme d’**arborescence**. Chaque processus (enfant) est créé par un autre processus (parent), et cette hiérarchie remonte au processus système `init` (ou `systemd`), qui est l'ancêtre de tous les processus.

### **Concept clé**  

- Le **PID** (*Process ID*) est un identifiant unique pour chaque processus.  
- Le **PPID** (*Parent Process ID*) identifie le parent du processus.  
- L’arborescence permet de visualiser les relations parent-enfant, ce qui est essentiel pour comprendre la gestion des processus.  

### **Analogie**  

Imaginez l’arborescence des processus comme un arbre généalogique :  

- `init` ou `systemd` est l'ancêtre commun (la racine).  
- Chaque parent peut avoir plusieurs enfants (branches).  
- Les feuilles représentent les processus terminaux, sans enfants.

## 2. Expérimentation guidée

### Préparation

1. **Ouvrir Terminator**  
   Divisez votre terminal en deux colonnes (cliquez droit → « Split vertically »).  
   - Terminal gauche : Lancez le programme de démonstration.  
   - Terminal droit : Explorez l’arborescence des processus.  

2. **Lancer le programme**  
   Dans le terminal gauche, exécutez le programme pédagogique pour créer une arborescence de processus :  

   ```bash
   procarbo
   ```

### Étape 1 : Observer les processus avec `ps`

1. **Lister les processus actifs**  
    Dans le terminal droit, utilisez la commande suivante pour afficher les processus en cours :  

    ```bash
    psess
    ```

    **Explications :**  
    - `psess` est un alias qui affiche les colonnes essentielles pour analyser les processus :  
        - `PPID` : PID du processus parent.  
        - `PID` : Identifiant du processus.  
        - `STAT` : État du processus (Running, Sleeping, etc.).  
        - `TTY` : Terminal associé au processus.  
        - `USER` : Utilisateur propriétaire.  
        - `CMD` : Commande utilisée pour démarrer le processus.  

    **Question :** Quels sont les PID et PPID des processus créés par le programme `procarbo` ?  

2. **Afficher l’arborescence des processus**  
    Pour visualiser les relations entre processus, utilisez la commande suivante :  

    ```bash
    ps --forest
    ```

    **Explications :**  
    - `--forest` : Ajoute une arborescence graphique en ASCII pour représenter les relations parent-enfant.  

    **Question :** Où se trouve le processus principal du programme `procarbo` dans l’arborescence ?  

### Étape 2 : Visualiser l’arborescence avec `pstree`

1. **Afficher l’arborescence complète**  
    Dans le terminal droit, utilisez l’alias `treeproc` :  

    ```bash
    treeproc
    ```

    **Explications :**  
    - `pstree` : Affiche l’arborescence complète des processus en cours.  
    - `-p` : Ajoute les PID à chaque processus.  
    - `-c` : Désactive le regroupement des processus similaires.  
    - `-l` : Affiche les noms longs sans les tronquer.  

    **Question :** Combien d’enfants et de petits-enfants le programme `procarbo` a-t-il créés ?  

2. **Afficher l’arborescence autour du shell courant**  
    Utilisez l’alias `stree` pour ne voir que les processus liés à votre shell :  

    ```bash
    stree
    ```

    **Question :** Quels processus sont directement liés à votre terminal ?  

### Étape 3 : Analyser les processus liés à Terminator

1. **Observer les processus enfants de Terminator**  
    Utilisez l’alias `termtree` pour afficher les processus enfants de Terminator :  

    ```bash
    termtree
    ```

    **Question :** Quels processus sont créés spécifiquement par Terminator ?  

### Étape 4 : Explorer l’arborescence avec `htop`

1. **Lancer `htop`**  
    Dans le terminal droit, exécutez :  

    ```bash
    htop
    ```

    **Explications :**  
    - `htop` : Outil interactif pour surveiller les processus.  
    - Utilisez la touche ++f5++ pour afficher les processus en mode arborescence.  
    - Naviguez avec les flèches pour explorer les relations parent-enfant.  

    **Question :** Quel est le PID de `procarbo` et comment ses enfants sont-ils affichés dans `htop` ?  

## 3. Exercices pratiques

### Exercice 1 : Analyser les relations parent-enfant

1. Relancez `procarbo`.  
2. Utilisez `psess`, `treeproc` et `ps --forest` pour analyser l’arborescence des processus.  

**Question :** Comment l’arborescence évolue lorsque vous terminez un processus enfant avec `kill` ?  

### Exercice 2 : Comprendre les états des processus

1. Observez la colonne `STAT` dans la commande `psess`.  
2. Identifiez les différents états (`S`, `R`, `Z`, etc.).  

**Question :** Que signifie chaque état ? Comment vérifier si un processus est en veille ou actif ?  

### Exercice 3 : Expérimenter avec plusieurs instances

1. Lancez plusieurs instances de `procarbo`.  
2. Utilisez `treeproc` et `psess` pour analyser les relations entre les processus.

**Question :** Comment distinguer les différentes instances et leurs enfants dans l’arborescence ?  

## 4. Tableau récapitulatif des commandes

| **Commande**  | **Alias**   | **Explication**                                             | **Cas d’utilisation**                                           |
|---------------|-------------|------------------------------------------------------------|-----------------------------------------------------------------|
| `ps -o ...`   | `psess`     | Affiche les colonnes essentielles des processus.           | Observer les PID, PPID, état et commande des processus actifs. |
| `ps --forest` | (aucun)     | Affiche les processus sous forme d’arborescence.           | Visualiser les relations parent-enfant.                        |
| `pstree`      | `treeproc`  | Affiche l’arborescence complète avec les PID.              | Explorer toutes les relations de processus.                    |
| `pstree $$`   | `stree`     | Affiche l’arborescence autour du shell courant.            | Analyser les processus directement liés au terminal.           |
| `pstree ...`  | `termtree`  | Affiche les enfants de Terminator.                         | Visualiser les processus créés par Terminator.                 |
| `htop`        | (aucun)     | Affiche un moniteur interactif des processus en arborescence. | Explorer et surveiller les processus en temps réel.            |

## 5. Résumé des points clés

- Les processus sont organisés en arborescence, chaque processus ayant un parent (`PPID`) et pouvant avoir des enfants (`PID`).  
- Des outils comme `ps`, `pstree` et `htop` permettent de visualiser et d’analyser cette hiérarchie.  
- Les alias simplifient l’utilisation des commandes complexes pour des tâches fréquentes.

## 6. Évaluation des connaissances

### **Questions à choix multiples**

**Q1 : Quelle commande affiche une arborescence des processus avec leurs PID ?**  

1. `psess`  
2. `pstree`  
3. `ps --forest`  

> **Réponse attendue :** 2.  

**Q2 : Quel alias permet d’afficher uniquement les processus enfants de Terminator ?**  

1. `treeproc`  
2. `stree`  
3. `termtree`  

> **Réponse attendue :** 3.  

**Q3 : Que signifie l’état `S` dans la colonne `STAT` des processus ?**  

1. Processus actif.  
2. Processus en veille.  
3. Processus zombie.  

> **Réponse attendue :** 2.  

