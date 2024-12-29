# **Scénario pédagogique : Découvrir le PID et l'état des processus**

## **Objectifs**

1. **Comprendre le rôle du PID** : Identifier un processus spécifique via son PID.
2. **Observer et comprendre les états des processus** (`R`, `S`, `T`).
3. **Découvrir et utiliser des commandes essentielles pour diagnostiquer les processus.**

---

## **Introduction pour les étudiants**

Présentez les concepts de **PID** et d’**état des processus** :

- **PID** : Chaque processus actif sur un système Linux a un identifiant unique appelé PID (Process ID).
- **États (`R`, `S`, `T`)** :
  - **`R` (Running)** : Le processus est actif et utilise le CPU.
  - **`S` (Sleeping)** : Le processus est inactif, en attente d’un événement.
  - **`T` (Stopped)** : Le processus est suspendu (volontairement, par un signal `STOP`).

**Le programme `procstate`** alterne entre deux états :

1. Une phase **BUSY** où il consomme activement le CPU (**état `R`**).
2. Une phase **SLEEP** où il attend (**état `S`**).

---

## **Déroulé pédagogique**

### **Étape 1 : Lancer le programme**

1. Dites aux étudiants de lancer `procstate` :

   ```bash
   ./procstate
   ```

2. **Question :**
   - Que voyez-vous dans le terminal ? (Le programme affiche son PID et alterne entre deux phases.)

3. Expliquez que le **PID affiché** peut être utilisé pour manipuler et observer le processus dans les manipulations qui vont suivre.

---

### **Étape 2 : Identifier le PID**

1. **Trouver le PID avec `pidof` :**
    Demandez aux étudiants d’utiliser la commande suivante :

    ```bash
    pidof procstate
    ```

    **Question :**
    - Quel est le PID du programme `procstate` ?

2. **Vérifier avec `ps` :**
    Les étudiants peuvent confirmer le PID et voir les détails du processus

    Pour faciliter l’analyse, vous pouvez créer un alias `psess` pour afficher les colonnes essentielles :

    ```bash
    # Affiche les processus avec les colonnes essentielles pour une analyse rapide :
    #   - PPID : PID du processus parent.
    #   - PID  : Identifiant du processus.
    #   - STAT : État du processus (Running, Sleeping, etc.).
    #   - TTY  : Terminal associé au processus (ou ? si aucun).
    #   - USER : Utilisateur propriétaire du processus.
    #   - CMD  : Commande utilisée pour démarrer le processus.
    alias psess='ps -o ppid,pid,stat,tty,user,cmd' # Affichage des processus avec les colonnes essentielles
    ```

    Expliquer la syntaxe de `$(pidof procstate)` pour remplacer le PID dans la commande `ps`.

    ```bash
    psess -p $(pidof procstate)
    ```

    **Question :**
    - Quel est l’état (`STAT`) du processus actuellement ? (Essayez pendant les phases BUSY et SLEEP.)

---

### **Étape 3 : Observer en temps réel avec `htop`**

1. Lancez `htop` en filtrant sur le programme `procstate` :

   ```bash
   htop -p $(pidof procstate)
   ```

2. **Actions :**
   - Regardez les variations dans l’utilisation CPU entre les phases BUSY et SLEEP.
   - Observez l’état (`R` ou `S`) du processus dans la colonne correspondante.

3. **Question :**
   - Que remarquez-vous sur l’utilisation CPU entre les phases ?

---

### **Étape 4 : Manipuler l'état avec des signaux**

1. Expliquez comment utiliser `kill` pour modifier l’état d’un processus.

2. **Suspendre le processus** :
   Demandez aux étudiants d’envoyer le signal `STOP` au processus :

   ```bash
   kill -STOP $(pidof procstate)
   ```

   Observez que le processus passe à l’état `T` (Stopped) avec :

   ```bash
   ps -o pid,stat,cmd -p $(pidof procstate)
   ```

3. **Reprendre le processus** :
   Demandez aux étudiants d’envoyer le signal `CONT` pour relancer le processus :

   ```bash
   kill -CONT $(pidof procstate)
   ```

   Vérifiez que le processus reprend son exécution, alternant entre `R` et `S`.

4. **Question :**
   - Quelle est la différence entre les états `T` (Stopped) et `S` (Sleeping) ?
   - Pourquoi le processus ne consomme-t-il pas de CPU en état `T` ?

---

## **Synthèse et conclusion**

1. **Révision des concepts :**
   - **PID** : Chaque processus a un identifiant unique, utilisé pour le diagnostiquer ou le manipuler.
   - **États (`R`, `S`, `T`)** :
     - `R` : Actif et utilise le CPU.
     - `S` : Inactif, en attente d’un événement.
     - `T` : Suspendu volontairement.

2. **Commandes apprises :**
   - `pidof` : Trouver le PID d’un processus à partir de son nom.
   - `ps` : Afficher les détails d’un processus spécifique.
   - `htop` : Observer les processus en temps réel.
   - `kill` : Envoyer des signaux pour manipuler un processus.

---

## **Commandes utilisées dans le scénario**

Les alias facilitent l’utilisation des commandes suivantes :

- **Trouver le PID** :

  ```bash
  pidof procstate
  ```

- **Observer un processus spécifique** :

  ```bash
  ps -o pid,stat,cmd -p $(pidof procstate)
  ```

- **Observer en temps réel** :

  ```bash
  htop -p $(pidof procstate)
  ```

- **Manipuler le processus** :
  - Suspendre : `kill -STOP $(pidof procstate)`
  - Reprendre : `kill -CONT $(pidof procstate)`

---

## **Pour aller plus loin**

Quelques signaux utiles

    SIGTERM : demande « poliment » au processus de s'arrêter

    SIGKILL : tue le processus (utile lorsque SIGTERM échoue, ce signal n'est pas « personnalisable »)

    SIGTSTP : suspend le processus

    SIGCONT : relance un processus suspendu

Remarques

    CTRL+C a pour effet d'envoyer un signal (SIGINT) au(x) processus courant(s).

    CTRL+Z a pour effet d'envoyer le signal SIGTSTP au processus courant.

    bg sert en fait à envoyer SIGCONT à un processus.

    fg envoie également SIGCONT au processus, et fait en sorte d'attendre la fin.

    On peut voir que man a personnalisé sa réponse à SIGINT (le procecuss n'est pas tué lorsqu'on tape CTRL+C).
