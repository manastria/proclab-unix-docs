J'ai annoté le document avec des commentaires TODO pour te demander des modifications.
Je te donne également les directives pour la rédaction
# Directives pour la rédaction de documents techniques pédagogiques en cybersécurité

### Structure et progression
Le document doit suivre une progression logique, en commençant par les concepts fondamentaux avant d'aborder les notions plus complexes. Chaque nouvelle notion doit s'appuyer sur les concepts précédemment introduits.

Présentation du vocabulaire technique
Lors de l'introduction d'un terme technique, le présenter ainsi :
- Le terme en français en **gras**
- Son équivalent anglais en *italique*
- Si pertinent, mentionner les appellations courantes ("plus connu sous le nom de...")
- Utiliser le vocabulaire technique
- Fournir immédiatement une définition claire, car les étudiants sont novices
- Illustrer avec un exemple concret d'utilisation
- Une fois le vocabulaire expliqué et les équivalents mentionnés, préférer les termes anglais dans le texte

### Visualisation des concepts
Inclure des schémas et diagrammes pour :
- Illustrer les architectures systèmes et réseaux
- Représenter les flux de données
- Visualiser les processus de sécurité
Utiliser la syntaxe Mermaid pour les diagrammes dans mkdocs.

### Gestion du code et des commandes
Pour les commandes Linux :
- Présenter la commande dans un bloc de code propre, sans commentaires
- Expliquer en détail la commande dans le texte qui suit
- Fournir des cas d'usage réels et leurs résultats attendus
- Mentionner les variations courantes de la commande

Pour les scripts :
- Inclure des commentaires explicatifs dans le code
- Structurer le code de manière claire avec des sections logiques
- Expliquer la logique générale du script dans le texte avant le bloc de code

### Mise en forme mkdocs
Utiliser les éléments de mise en forme spécifiques :
- Callouts pour les points importants, avertissements et astuces
- Callouts pliables pour les sections détaillées ou les explications approfondies
- Mettre en forme les raccourcis clavier avec la syntaxe `++ctrl+shift+e++`
- Utiliser des guillemets français dans le texte.

### Pédagogie et progression
À la fin de chaque section majeure :
- Résumer les points clés
- Proposer des exercices pratiques simples
- Établir des liens avec les concepts qui seront abordés dans les sections suivantes





# Scénario 3 : Explorer les états des processus

Imaginez que vous êtes un détective qui observe le comportement des processus sur votre système. Dans ce scénario, nous allons découvrir comment les processus "vivent" dans Linux, quels états ils peuvent prendre, et comment nous pouvons les observer et les influencer.

## Objectifs d'apprentissage

À la fin de ce scénario, vous serez capable de :

- Comprendre ce qu'est l'état d'un processus et pourquoi il change
- Utiliser des outils pour observer ces états
- Manipuler l'état d'un processus de manière basique

## Préparation : Notre laboratoire d'observation

Pour mener nos expériences, nous allons utiliser deux outils :

1. **Le programme procstate** : Un programme spécial qui va nous montrer de manière visible les différents états d'un processus. C'est comme une petite souris de laboratoire qui va nous aider à comprendre le comportement des processus.

2. **Terminator** : Notre "laboratoire" avec deux zones d'observation :
   - À gauche : pour exécuter notre programme
   - À droite : pour l'observer avec différents outils

### Mise en place du laboratoire

1. Ouvrez Terminator
2. Appuyez sur ++ctrl+shift+e++ pour diviser la fenêtre verticalement
3. Vous avez maintenant deux terminaux côte à côte

## Partie 1 : Premiers pas dans l'observation des processus

### Le PID : La carte d'identité d'un processus

Avant de commencer nos observations, nous devons comprendre comment identifier un processus. Sous Linux, chaque processus reçoit un numéro unique appelé PID (*Process IDentifier*). Il est unique et permet de les identifier sans ambiguïté.

Dans le terminal de gauche, lancez notre programme d'observation :

```bash
./procstate
```

Vous devriez voir quelque chose comme :

```
[Début] PID = 1234
Ce programme alterne 5 s de calcul intensif (BUSY) et 3 s de sommeil (SLEEP).
```

// TODO: Expliquer comment terminer le programme avec CTRL+C. C'est une information, ne pas le faire.

!!! question "Réflexion initiale"
    1. Notez le PID affiché. Est-il le même à chaque lancement ? //TODO: Pour répondre à cette question, l'étudiant devra le faire en marge des activités actuelles. Il faudra expliquer comment le faire sinon il sera dérouté.
    2. Pourquoi pensez-vous que chaque processus a besoin d'un identifiant unique ?

### À la recherche du PID perdu

Si vous perdez le PID de vue ou si vous lancez plusieurs instances du programme, Linux fournit un outil très pratique pour retrouver les PID : la commande `pidof`. Elle fait exactement ce que son nom suggère - elle trouve le PID d'un programme à partir de son nom.

```bash
pidof procstate
```

// TODO: Préciser que la commande doit être exécutée dans le terminal de droite.

!!! note "Comment fonctionne pidof"
    La commande `pidof` cherche dans la liste des processus en cours d'exécution et affiche les PID des processus qui correspondent au nom donné. C'est comme demander : "Qui s'appelle procstate parmi les processus en cours ?"
    
    Si vous lancez plusieurs fois le même programme, `pidof` affichera plusieurs PID, un pour chaque instance en cours d'exécution.

### La magie de la substitution de commande

Dans le shell Linux, nous pouvons utiliser le résultat d'une commande directement dans une autre commande. C'est ce qu'on appelle la substitution de commande, et elle se fait avec la syntaxe `$(commande)`.

Essayons de comprendre avec un exemple simple :

// TODO: Préciser à faire dans le terminal de droite.

```bash
echo "Bonjour, nous sommes le $(date)"
```

Dans cette commande :

1. Le shell exécute d'abord la commande entre `$( )`
2. Il remplace ensuite toute l'expression `$(date)` par son résultat
3. Finalement, il exécute la commande `echo` avec le texte substitué

C'est comme si vous disiez au shell : "Là où tu vois `$(commande)`, remplace-le par ce que cette commande afficherait si tu l'exécutais."

// TODO: attention, il faut que procstate soit lancé dans le terminal de gauche.

!!! exercise "Exercice : Comprendre la substitution"
    1. Exécutez ces deux commandes et comparez leurs résultats :
       ```bash
       echo "Le PID est $(pidof procstate)"
       echo "Le PID est pidof procstate"
       ```
    2. Quelle est la différence ? Pourquoi ?
    3. Inventez votre propre commande utilisant `$(...)` avec une autre commande Linux que vous connaissez.

## Partie 2 : Observer les états des processus

### Les états : Le cycle de vie d'un processus

Un processus n'est pas toujours en train de calculer. Comme nous les humains, il peut être dans différents états :

1. **En cours d'exécution** (*Running* - `R`) : 
   - Le processus est actuellement en train de calculer
   - Comme quand vous êtes concentré sur un calcul mental

2. **En sommeil** (*Sleeping* - `S`) :
   - Le processus attend quelque chose (une entrée, un timer...)
   - Comme quand vous attendez le bus - vous ne faites rien mais vous êtes prêt à réagir

3. **Suspendu** (*Stopped* - `T`) :
   - Le processus est temporairement arrêté
   - Comme quand vous mettez une vidéo en pause

```mermaid
stateDiagram-v2
    [*] --> Ready : Création
    Ready --> Running : Sélection
    Running --> Ready : Préemption
    Running --> Sleeping : Attente E/S
    Sleeping --> Ready : E/S terminée
    Running --> Stopped : Signal STOP
    Stopped --> Ready : Signal CONT

    note right of Ready : Prêt à s'exécuter
    note right of Running : En train de calculer
    note right of Sleeping : En attente
    note right of Stopped : En pause
```

Ce diagramme montre comment un processus passe d'un état à l'autre :

- Quand il est créé, il est d'abord prêt à s'exécuter
- Le système le sélectionne pour s'exécuter (état *Running*)
- S'il doit attendre quelque chose, il passe en *Sleeping*
- On peut le mettre en pause (*Stopped*) et le reprendre plus tard

### Notre outil d'observation : L'alias psess

Pour observer facilement les processus, nous allons créer un outil personnalisé. Sous Linux, on peut créer des raccourcis de commande appelés "alias". Voici un alias très utile pour observer les processus :

```bash
# Affiche les processus avec les colonnes essentielles pour une analyse rapide :
#   - PPID : PID du processus parent.
#   - PID  : Identifiant du processus.
#   - STAT : État du processus (Running, Sleeping, etc.).
#   - TTY  : Terminal associé au processus (ou ? si aucun).
#   - USER : Utilisateur propriétaire du processus.
#   - CMD  : Commande utilisée pour démarrer le processus.
alias psess='ps -o ppid,pid,stat,tty,user,cmd'
```

C'est comme créer notre propre loupe d'observation, spécialement adaptée pour voir ce qui nous intéresse dans les processus.

!!! tip "Conseil pratique"
    Pour garder cet outil toujours disponible, ajoutez cette ligne dans votre fichier `~/.bashrc`. 
    C'est comme ranger notre loupe dans un tiroir facilement accessible !

### Observer notre processus

Maintenant que nous avons notre outil, observons notre processus :

```bash
psess -p $(pidof procstate)
```

!!! exercise "Exercice d'observation"
    Observez pendant au moins 30 secondes et notez : // TODO: Je ne comprends pas pourquoi observer pendant 30 secondes. C'est quoi l'objectif ? ps n'est pas htop, il affiche un résultat et il n'évoluera pas.
    1. Les différents états que vous voyez dans la colonne STAT
    2. À quel moment de l'affichage du programme ils correspondent
    3. Le PPID (Process Parent ID) - qui est le "parent" de notre processus ?

Analysons chaque colonne de la sortie :

- PPID : Le PID du processus qui a créé celui-ci (généralement votre shell)
- PID : L'identifiant unique de notre processus
- STAT : L'état actuel (R, S, ou T)
- TTY : Le terminal associé
- USER : Qui a lancé le processus
- CMD : La commande qui tourne

### Observation en temps réel avec htop

Pour une vue plus dynamique, nous pouvons utiliser htop :

```bash
htop -p $(pidof procstate)
```

htop nous montre :

- L'utilisation CPU en temps réel
- L'état du processus qui change
- La mémoire utilisée
- Et bien d'autres informations

!!! tip "Navigation dans htop"
    - ++f4++ : Filtre pour voir uniquement certains processus // TODO: Info utile, mais pas nécessaire dans l'absolu. `htop -p` ne montre qu'un seul processus... Mais c'est une bonne info pour la suite.
    - ++q++ : Quitter htop
    - Les barres de couleur montrent l'intensité d'utilisation du CPU

## Partie 3 : Premiers pas dans la manipulation des processus

Maintenant que nous savons observer, essayons d'influencer le comportement de notre processus.

!!! warning "Note importante"
    Dans ce scénario, nous allons simplement voir comment "mettre en pause" un processus. Les signaux et la commande kill seront expliqués en détail dans les prochains scénarios. Pour l'instant, considérez les commandes suivantes comme des "formules magiques" que nous comprendrons mieux plus tard.

### Mettre un processus en pause

// TODO: Pourquoi ne pas utiliser CTRL+Z, jobs et fg/bg ? C'est commandes ont été vu dans le scénario précédent. Je trouve par contre, la commande kill plus rapide à cause de pidof. C'est donc préférable d'utiliser kill pour ce scénario. Mais, il faut expliquer. CTRL+Z peut être demandé en exercice.

Pour suspendre temporairement notre processus :

```bash
kill -STOP $(pidof procstate)
```

Vérifiez immédiatement l'état :

```bash
psess -p $(pidof procstate)
```

Vous devriez voir l'état 'T' (Stopped).

### Reprendre l'exécution

Pour faire reprendre le processus :

```bash
kill -CONT $(pidof procstate)
```

!!! exercise "Exercice final de synthèse"
    1. Lancez deux instances de procstate dans des terminaux différents
    2. Observez leurs états avec psess
    3. Mettez-en un en pause avec kill -STOP
    4. Que remarquez-vous ?
        - L'autre instance continue-t-elle de fonctionner ?
        - Les PID sont-ils différents ?
        - Les états sont-ils indépendants ?

## Conclusion et prochaines étapes

Nous avons découvert :

- Comment identifier un processus avec son PID
- Les différents états qu'un processus peut prendre
- Comment observer ces états
- Comment mettre en pause et reprendre un processus

Dans les prochains scénarios, nous approfondirons :

- Les signaux et la commande kill en détail
- Les états spéciaux comme les zombies
- Et bien d'autres aspects passionnants des processus Linux !

!!! question "Questions de réflexion finale"
    1. Pourquoi un processus passe-t-il en état Sleeping ?
    2. Un processus en état Stopped consomme-t-il du CPU ?
    3. Que pourrait-il se passer si on ferme le terminal d'un processus suspendu ?
