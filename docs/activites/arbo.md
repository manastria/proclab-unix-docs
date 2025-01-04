# Scénario : Arborescence des processus

## Objectifs d'apprentissage

À la fin de ce scénario, vous serez capable de :

- Comprendre ce qu'est une arborescence de processus
- Visualiser les relations parent-enfant entre les processus
- Utiliser les commandes `ps`, `pstree` et `htop` pour observer l'arborescence
- Identifier le parent d'un processus donné

## Concepts fondamentaux

### Les identifiants de processus

Sous Linux, chaque processus est identifié par deux numéros importants :

- Le **PID** (*Process ID*) est un identifiant unique pour chaque processus.

  ```bash
  # Exemple : chaque processus a un PID différent
  PID 1234 : firefox
  PID 1235 : terminal
  PID 1236 : bash
  ```

- Le **PPID** (*Parent Process ID*) identifie le parent du processus.

  ```bash
  # Exemple : le PPID indique quel processus est le parent
  PID 1236 (bash) → PPID 1235 (terminal)
  ```

!!! tip "Pour bien comprendre"
    - Le PID, c'est comme votre numéro d'étudiant : il est unique et vous identifie.
    - Le PPID, c'est comme le numéro de votre classe : il indique à quel groupe vous appartenez.

### Qu'est-ce qu'une arborescence de processus ?

Une **arborescence de processus** (*process tree*) est la représentation des relations entre les processus sous Linux, où chaque processus est créé par un autre processus (son parent).

!!! example "L'analogie de l'entreprise"
    Imaginez une entreprise avec son organigramme :

    - Le PDG (comme le processus `init` ou `systemd`) est au sommet
    - Les managers (processus parents) supervisent leurs équipes
    - Les employés (processus enfants) travaillent sous la direction des managers
    - Chaque employé sait qui est son responsable (comme le PPID - Parent Process ID)

    **Question de réflexion :** En quoi cette hiérarchie aide-t-elle à organiser le travail ?

    **Indices pour la réflexion :**
    - Que se passe-t-il quand un employé a un problème ? À qui s'adresse-t-il ?
    - Comment le PDG sait-il ce qui se passe dans l'entreprise ?
    - Pourquoi est-il important de savoir qui est responsable de qui ?

### Les concepts clés

1. **Le processus init/systemd (PID 1)**
    - C'est l'ancêtre de tous les processus
    - Il démarre automatiquement au démarrage du système
    - Il adopte les processus orphelins

2. **La relation parent-enfant**
    - Chaque processus (sauf `init`) a un parent
    - Un processus peut créer plusieurs enfants
    - On parle de "fork" quand un processus crée un enfant



!!! question "Questions préliminaires"
    Avant de commencer les manipulations, réfléchissons :

    1. Pourquoi est-il important de connaître les relations entre les processus ?

        **Pour vous aider :**

        - Dans une famille, pourquoi est-il important de savoir qui sont les parents de chaque enfant ?
        - Si un parent ne rentre pas à la maison, qui doit s'occuper des enfants ?
        - Quand un processus a un problème, pourquoi est-il utile de savoir qui l'a créé ?

        **Exemple concret :**

        Quand une application plante, le système doit :

        - Informer le processus parent du problème
        - Permettre au parent de gérer la situation
        - S'assurer qu'aucune ressource n'est perdue

    2. Que se passe-t-il quand un processus parent se termine ?

        **Indices :**

        - Que devient une équipe quand son manager quitte l'entreprise ?
        - Qui doit s'occuper des "employés" restants ?





## Mise en pratique

### Préparation

1. Ouvrez deux terminaux côte à côte dans Terminator (++ctrl+shift+e++)
2. Terminal gauche : pour lancer les programmes
3. Terminal droit : pour observer l'arborescence

### Étape 1 : Observer une arborescence simple

1. Dans le terminal gauche, lancez :
    ```bash
    procarbo --depth 2 --width 2
    ```

    Les options permettent de limiter la profondeur et la largeur de l'arborescence.

2. Dans le terminal droit, observez avec :
    ```bash
    ps --forest
    ```

!!! exercise "Exercice d'observation"
    1. Observez la sortie de `ps --forest`. Que représentent les caractères `─┬─` et `└─` ?

        **Aide visuelle :**
        ```
        parent1─┬─enfant1
                ├─enfant2
                └─enfant3

        parent2───enfant_unique
        ```

        **Comment lire cette représentation :**

        - Le caractère `┬` indique que le processus parent a plusieurs enfants
        - Le caractère `├` montre qu'il y a d'autres enfants qui suivent
        - Le caractère `└` indique le dernier enfant de ce parent
        - Les lignes `─` relient un parent à ses enfants

        **Exemple concret :**

        ```
        firefox─┬─firefox (onglet 1)
                ├─firefox (onglet 2)
                └─firefox (onglet 3)
        ```

        Ici, le processus Firefox principal a créé trois processus enfants, un pour chaque onglet.

    2. Combien de niveaux de processus voyez-vous ?

        **Pour compter les niveaux :**

        - Commencez par le processus parent
        - Suivez les lignes vers la droite
        - Chaque décalage représente un nouveau niveau

### Étape 2 : Explorer avec pstree

La commande `pstree` (*Process Tree*) affiche les processus sous forme d'arbre. L'option `-p` ajoute les PIDs entre parenthèses.

Lancez la commande :
```bash
pstree -p
```

**Exemple de sortie :**
```text { .custom-code }
systemd(1)─┬─ModemManager(889)─┬─{ModemManager}(904)
           │                   └─{ModemManager}(906)
           ├─agetty(890)
           ├─sshd(891)───sshd(23617)───bash(23619)───pstree(23620)
           └─terminator(12567)─┬─bash(12575)───procarbo(12898)
                               └─bash(12576)
```

**Comment lire cette sortie :**

1. L'arbre commence toujours par `systemd(1)` qui est le processus initial
   - Le chiffre 1 est son PID
   - Tout part de ce processus racine

2. Les traits représentent les relations :
   - `┬` signifie "a plusieurs enfants"
   - `├` signifie "un enfant, et il y en a d'autres"
   - `└` signifie "dernier enfant"
   - `─` connecte un parent à son enfant

3. Dans cet exemple :
   - `ModemManager(889)` gère le modem avec deux processus légers (threads)
   - `sshd(891)` gère les connexions SSH
   - `terminator(12567)` est notre terminal avec deux shells bash

!!! info "Processus légers (threads)"
    Les accolades comme dans `{ModemManager}` indiquent des processus légers (threads).
    Ce sont des sous-processus qui partagent les ressources de leur parent.

!!! tip "Astuce"
    Vous pouvez filtrer l'affichage en ajoutant le PID d'un processus spécifique :
    ```bash
    pstree -p <PID>
    ```
    Cela montre uniquement l'arbre à partir de ce processus.

!!! exercise "Analyse de l'arborescence"
    1. Que signifient les nombres entre parenthèses ?

        **Indice :** Comparez avec les PID affichés par `ps`

    2. Pourquoi certains processus apparaissent-ils plusieurs fois ?

        **Pour comprendre :**
        - Pensez à un employé qui travaille dans plusieurs équipes
        - Est-ce le même processus ou des processus différents ?

1. **Lister les processus actifs**
    Dans le terminal droit, utilisez la commande suivante pour afficher les processus en cours :

    ```bash
    psess
    ```

    <!-- TODO: Indiquer aux étudiants d'où vient l'alias `psess` -->

    **Explications :**
    - `psess` est un alias qui affiche les colonnes essentielles pour analyser les processus :
        - `PPID` : PID du processus parent.
        - `PID` : Identifiant du processus.
        - `STAT` : État du processus (Running, Sleeping, etc.).
        - `TTY` : Terminal associé au processus.
        - `USER` : Utilisateur propriétaire.
        - `CMD` : Commande utilisée pour démarrer le processus.

    **Question :** Quels sont les PID et PPID des processus créés par le programme `procarbo` ?



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

## Points clés à retenir

- Tout processus (sauf init/systemd) a un parent
- L'arborescence permet de visualiser les relations entre processus
- Les processus sont organisés en arborescence, chaque processus ayant un parent (`PPID`) et pouvant avoir des enfants (`PID`).
- Des outils comme `ps --forest`, `pstree` et `htop` permettent de visualiser et d’analyser cette hiérarchie.
- Les alias simplifient l’utilisation des commandes complexes pour des tâches fréquentes.
- Le PID 1 (init/systemd) est la racine de l'arbre

## Évaluation des connaissances

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

Q3. Qu'est-ce qu'un processus parent ?
    - [ ] Un processus très ancien
    - [ ] Un processus qui en crée d'autres
    - [ ] Le premier processus du système

    **Indice :** Pensez à la relation manager-employé

Q4. Quel est le rôle de init (PID 1) ?

    **Pour réfléchir :**
    - Que se passe-t-il au démarrage du système ?
    - Qui s'occupe des processus orphelins ?


## Lien avec les notions futures

Dans les prochains scénarios, nous verrons :
- Les processus zombies et orphelins
- La communication entre processus
- La gestion des signaux entre processus

!!! tip "Préparation pour la suite"
    Réfléchissez à :
    - Que se passe-t-il quand un processus parent meurt avant ses enfants ?
    - Comment les processus peuvent-ils communiquer entre eux ?
