# Scénario : Signaux

---

## Objectif pédagogique

Découvrir et expérimenter le fonctionnement des **signaux** sous Linux.  
Les étudiants apprendront à identifier, envoyer et intercepter des signaux à l'aide d'un programme interactif écrit en C et de commandes comme `kill` et `stty`.

---

## 1. Introduction : Qu’est-ce qu’un signal sous Linux ?

### **Définition**  

Un **signal** est un mécanisme utilisé par Linux pour envoyer une notification à un processus.  
Les signaux permettent de **communiquer avec un processus**, qu’il soit au premier plan ou en arrière-plan. Ils peuvent, par exemple :  
- Arrêter un processus, comme avec `CTRL+C`.  
- Mettre un processus en pause ou le reprendre.  
- Envoyer une commande utilisateur pour effectuer une tâche spécifique.

---

### **Concept clé**  

Les signaux sont identifiés par un nom (par exemple, `SIGINT`) et un numéro (par exemple, `2`).  
- Certains signaux sont générés par le système (comme `SIGHUP` lorsque vous fermez un terminal).  
- D'autres peuvent être envoyés manuellement par un utilisateur, via la commande `kill` ou des touches clavier.  

---

### **Analogie**  
Pensez aux signaux comme des messages envoyés à un employé (le processus) :  
- `SIGINT` : « Arrête ce que tu fais immédiatement ! »  
- `SIGUSR1` : « Voici une tâche spéciale à effectuer. »  
- `SIGKILL` : « Je suis le patron, tu es viré immédiatement. »  

---

## 2. Expérimentation guidée

### Préparation

1. **Ouvrir Terminator**  
   Divisez votre terminal en deux colonnes (cliquez droit → « Split vertically »).  
   - Terminal gauche : Lancez le programme interactif.  
   - Terminal droit : Envoyez des signaux au programme.  

2. **Lancer le programme**  
   Dans le terminal gauche :  
   ```bash
   ./signal_demo
   ```

---

### Étape 1 : Découvrir les signaux envoyés par le clavier

1. **Utiliser `stty` pour explorer les raccourcis clavier**  
   Dans le terminal gauche :  
   ```bash
   stty -a
   ```

   #### **Explications :**  
   - `stty -a` : Affiche les configurations du terminal, notamment les raccourcis clavier pour envoyer des signaux.  
   - Recherchez les lignes comme :  
     ```
     intr = ^C; quit = ^\; susp = ^Z
     ```
     - `intr` (`^C`) : Interrompt un processus (envoie `SIGINT`).  
     - `quit` (`^\`) : Arrête un processus et génère un vidage mémoire (envoie `SIGQUIT`).  
     - `susp` (`^Z`) : Met un processus en pause (envoie `SIGTSTP`).

   **Question : Quels signaux peuvent être envoyés par le clavier ?**  
  > **Indice :** Regardez les noms et raccourcis associés dans la sortie de la commande `stty -a`. Par exemple, `intr = ^C` correspond à `CTRL+C`, qui envoie un signal.  

1. **Tester les signaux depuis le clavier**  
   - Dans le terminal gauche, appuyez sur `CTRL+C`. Que se passe-t-il ?  
   - Appuyez sur `CTRL+Z`.  
   - Essayez `CTRL+\`.  

---

#### **Question d’observation**  
- Que fait le programme en réponse à ces signaux ?  
- Pourquoi certains signaux (comme `CTRL+\`) provoquent-ils une action plus brutale que d’autres ?

---

### Étape 2 : Envoyer des signaux avec la commande `kill`

1. **Identifier le processus actif**  
   Dans le terminal droit, utilisez `ps` pour lister les processus en cours :  
   ```bash
   ps -ef | grep signal_demo
   ```

   #### **Explications :**  
   - `ps -ef` : Affiche tous les processus actifs avec des informations détaillées.  
   - `grep signal_demo` : Filtre la liste pour ne montrer que les processus liés au programme.  

   **Indice :** Notez le **PID** (identifiant) du processus `signal_demo`.

2. **Envoyer des signaux au processus**  
   - Envoyez le signal `SIGUSR1` pour demander un nombre aléatoire :  
     ```bash
     kill -USR1 <PID>
     ```
   - Envoyez le signal `SIGUSR2` pour une citation humoristique :  
     ```bash
     kill -USR2 <PID>
     ```
   - Essayez d’envoyer `SIGTERM` :  
     ```bash
     kill -TERM <PID>
     ```

    **Question : Comment le programme réagit-il à chaque signal ?**  
    > **Indice :** Observez les messages affichés dans le terminal gauche après avoir envoyé les signaux (`SIGUSR1`, `SIGUSR2`, etc.). Chaque signal déclenche une action spécifique.  

---

### Étape 3 : Comprendre les signaux non interceptables

1. **Tester SIGKILL et SIGSTOP**  
   - Envoyez le signal `SIGKILL` :  
     ```bash
     kill -9 <PID>
     ```
   - Relancez le programme, puis envoyez `SIGSTOP` :  
     ```bash
     kill -STOP <PID>
     ```

   #### **Explications :**  
   - `SIGKILL` : Termine immédiatement le processus, sans possibilité d’interception.  
   - `SIGSTOP` : Met un processus en pause, également sans possibilité d’interception.  

   **Question : Pourquoi ces signaux (SIGKILL, SIGSTOP) ne peuvent-ils pas être interceptés ?**  
  > **Indice :** Ces signaux sont gérés directement par le noyau Linux pour garantir qu’ils soient appliqués sans possibilité de modification par le processus. Réfléchissez à des situations où il serait dangereux de laisser un programme ignorer ces signaux.  

---

### Étape 4 : Expérimenter avec les signaux utilisateur

1. Dans le terminal droit, envoyez plusieurs fois `SIGUSR1` et `SIGUSR2`.  
2. Observez les résultats.  

**Question : Quels sont les cas d’utilisation concrets pour ces signaux ?**  
  > **Indice :** Dans le programme interactif, voyez comment `SIGUSR1` génère un nombre aléatoire ou comment `SIGUSR2` affiche une citation. Imaginez comment ces signaux pourraient être utilisés dans un programme réel.  


---

## 3. Tableau récapitulatif des signaux

| **Signal**  | **Nom pour `kill`** | **Numéro** | **Explication**                                      | **Cas d’utilisation**                                  |
|-------------|----------------------|------------|------------------------------------------------------|-------------------------------------------------------|
| SIGINT      | `-INT`              | 2          | Interrompt un processus en premier plan.             | Arrêter un processus via `CTRL+C`.                   |
| SIGTERM     | `-TERM`             | 15         | Demande à un processus de s’arrêter proprement.      | Fermer un programme en cours d’exécution.            |
| SIGKILL     | `-KILL`             | 9          | Force l’arrêt immédiat d’un processus (non interceptable). | Terminer un programme bloqué.                        |
| SIGSTOP     | `-STOP`             | (non assigné) | Met un processus en pause (non interceptable).       | Suspendre temporairement un processus.               |
| SIGUSR1     | `-USR1`             | 10         | Signal utilisateur 1 (défini par le programme).      | Personnalisation : ici, génère un nombre aléatoire.  |
| SIGUSR2     | `-USR2`             | 12         | Signal utilisateur 2 (défini par le programme).      | Personnalisation : ici, affiche une citation humoristique. |
| SIGQUIT     | `-QUIT`             | 3          | Interrompt un processus avec vidage mémoire.         | Arrêter un programme avec diagnostic.                |

---

## 4. Résumé des points clés

- Les **signaux** permettent de communiquer avec les processus pour les interrompre, les suspendre ou leur demander une tâche.  
- Les signaux peuvent être envoyés via le clavier (`CTRL+C`, `CTRL+Z`) ou la commande `kill`.  
- Certains signaux, comme `SIGKILL` et `SIGSTOP`, ne peuvent pas être interceptés par les programmes.  
- Les signaux utilisateurs (`SIGUSR1`, `SIGUSR2`) permettent de définir des actions personnalisées.

---

## 5. Évaluation des connaissances

### **Questions à choix multiples**

**Q1 : Quels signaux ne peuvent pas être interceptés par un programme ?**  
1. SIGTERM  
2. SIGKILL  
3. SIGUSR1  
4. SIGSTOP  

> **Indice :** Ces signaux ne laissent aucune chance au processus de réagir et sont toujours appliqués immédiatement.  

> **Réponses attendues :** 2 et 4.  

---

**Q2 : Que fait `SIGINT` lorsqu’il est envoyé à un processus ?**  
1. Met le processus en pause.  
2. Force l’arrêt immédiat du processus.  
3. Demande une interruption propre.  
4. Interrompt le processus avec possibilité de gestion.  

> **Indice :** C’est le signal que vous envoyez en appuyant sur `CTRL+C`. Que se passe-t-il dans le programme interactif ?  

> **Réponse attendue :** 4.  

---

**Q3 : Pourquoi `SIGUSR1` et `SIGUSR2` sont-ils utiles ?**  
1. Ils permettent de définir des actions personnalisées.  
2. Ils sont indispensables au fonctionnement du système.  
3. Ils forcent un processus à redémarrer.  

> **Indice :** Regardez ce que fait le programme quand vous envoyez ces signaux : le comportement est défini par le programme lui-même.  

> **Réponse attendue :** 1.  

---

Ces indices sont conçus pour guider les étudiants tout en les encourageant à relier les questions à leurs expérimentations pratiques.
---

## 6. Lien avec les notions futures

- **Processus zombies** : Les signaux comme `SIGKILL` peuvent provoquer des zombies si le parent ne récupère pas l’état du processus enfant.  
- **Processus orphelins** : L’envoi de signaux peut également être utilisé pour manipuler les processus parent/enfant.  
- **Scripts d’administration système** : Automatiser l’envoi et la gestion des signaux dans des scripts bash.
