# Scénario 1 : Terminal et processus

Dans ce premier scénario, nous allons explorer la relation entre les terminaux et les processus sous Linux. Vous découvrirez comment les processus sont liés à leur terminal de contrôle et comment observer ces relations.

## Objectifs d'apprentissage

À la fin de ce scénario, vous serez capable de :

- Identifier le terminal associé à un processus
- Comprendre la notion de terminal de contrôle
- Maîtriser les options de base de la commande `ps`

## Concepts fondamentaux

### Qu'est-ce qu'un processus ?

Un **processus** (*process*) est un programme en cours d'exécution. Mais cette définition technique ne suffit pas à comprendre ce concept essentiel. Prenons une première analogie simple pour commencer :

Dans un restaurant, chaque table occupée représente un processus différent :

- La table a son propre espace (la mémoire allouée au processus)
- Les clients ont leur propre commande (les instructions du programme)
- Chaque table a un numéro unique (comme le PID - *Process IDentifier*)

!!! note "Une analogie qui va s'enrichir"
    Cette image du restaurant va s'enrichir au fil des scénarios. Nous verrons comment l'organisation complète d'un restaurant (serveurs, maître d'hôtel, chef de salle...) nous aide à comprendre la gestion des processus dans son ensemble. Si vous êtes curieux, vous pouvez déjà consulter [l'analogie complète du restaurant](../ressources/analogie-restaurant.md), mais ne vous inquiétez pas si certains concepts ne sont pas encore clairs - nous les découvrirons progressivement.

En informatique, lorsque vous lancez un programme comme `firefox` ou notre `proclab`, le système d'exploitation :

1. Crée un nouvel espace en mémoire
2. Attribue un numéro unique (PID) au processus
3. Charge le programme dans cet espace
4. Commence son exécution

!!! example "Exemple concret"
    Quand vous lancez trois fois `proclab`, vous créez trois processus distincts :
    - Chacun a son propre PID
    - Chacun s'exécute indépendamment
    - Chacun a sa propre zone mémoire

```mermaid
graph TD
    A[Programme proclab] -->|Lancement 1| B[Processus PID 1234]
    A -->|Lancement 2| C[Processus PID 1235]
    A -->|Lancement 3| D[Processus PID 1236]
```

### Le terminal de contrôle

Un **terminal de contrôle** (*controlling terminal*) est l'interface par laquelle un utilisateur interagit avec un processus. Sous Linux, chaque processus est généralement associé à un terminal qui lui permet de :

- Recevoir les entrées de l'utilisateur (clavier)
- Afficher ses sorties (écran)
- Recevoir des signaux de contrôle (comme ++ctrl+c++)

!!! info "Les terminaux physiques et virtuels"
    Historiquement, un terminal était un appareil physique (comme un téletype). Aujourd'hui, nous utilisons des **terminaux virtuels** (*virtual terminals*) ou des **émulateurs de terminal** (*terminal emulators*) qui simulent le comportement des terminaux physiques.

## Mise en pratique

### Préparation de l'environnement

1. Lancez Terminator et divisez la fenêtre en deux parties verticales (++ctrl+shift+e++)
2. Dans chaque partie, vous aurez un terminal distinct avec son propre identifiant

### Étape 1 : Identifier son terminal

Dans chaque terminal, exécutez la commande :

```bash
tty
```

Cette commande affiche le chemin du périphérique terminal. Vous obtiendrez quelque chose comme `/dev/pts/0` pour le premier terminal et `/dev/pts/1` pour le second.

!!! tip "Comprendre la sortie"
    Le préfixe `/dev/pts/` indique qu'il s'agit d'un pseudo-terminal (*pseudo-terminal slave*), une émulation logicielle d'un terminal physique.

### Étape 2 : Observer les processus

Lancez `proclab` dans le premier terminal :

```bash
proclab
```

Dans le second terminal, exécutez :

```bash
ps -f
```

Observez la colonne `TTY` dans la sortie. Elle indique le terminal de contrôle de chaque processus. 

```
UID        PID  PPID  C TTY      STAT TIME CMD
user     15640 15639  0 pts/0    S+   0:00 proclab
user     15642 15641  0 pts/1    R+   0:00 ps -f
```

### Étape 3 : Comparer les vues des processus

Exécutez maintenant dans chaque terminal :

```bash
ps
```

!!! note "Différence avec ps -f"
    Sans option, `ps` ne montre que les processus associés au terminal courant.
    Avec l'option `-f` (*full-format*), il affiche tous les processus avec des informations détaillées.

## Visualisation de la relation terminal-processus

```mermaid
graph TD
    A[Terminal pts/0] --> B[proclab]
    C[Terminal pts/1] --> D[shell]
    D --> E[ps]
    style A fill:#f9f,stroke:#333
    style C fill:#f9f,stroke:#333
```

## Points clés à retenir

La relation entre un terminal et ses processus est fondamentale sous Unix :

1. Chaque terminal a un identifiant unique (`/dev/pts/X`)
2. Les processus héritent du terminal de leur parent
3. La commande `ps` permet d'observer ces relations
4. Un terminal peut contrôler plusieurs processus

## Pour aller plus loin

Essayez ces manipulations supplémentaires :

1. Lancez plusieurs instances de `proclab` dans le même terminal
2. Observez la colonne `TTY` avec `ps -f`
3. Comparez avec un `ps` simple

## Prochaine étape

Maintenant que vous maîtrisez la relation entre terminaux et processus, vous êtes prêt à explorer la [gestion des processus en avant-plan et en arrière-plan](fg-bg.md).

!!! tip "En cas de difficulté"
    N'oubliez pas que vous pouvez utiliser ++ctrl+c++ pour arrêter `proclab` à tout moment.
