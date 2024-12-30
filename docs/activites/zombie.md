# Activité pédagogique : Comprendre les processus zombie

---

## Objectif pédagogique

Apprendre à identifier, comprendre et manipuler un **processus zombie** (*zombie process*) sous Linux par l’expérimentation.  
Cette activité permet également de découvrir et d'utiliser des commandes essentielles pour la gestion des processus sous Linux.

---

## 1. Introduction : Qu’est-ce qu’un processus zombie ?

### **Définition**  

Un **processus zombie** (*zombie process*), parfois appelé processus mort-vivant, est un processus terminé dont l'entrée dans la table des processus n'a pas été supprimée parce que son **processus parent** n'a pas encore récupéré son état de sortie avec la commande système `wait()`.

---

### **Concept clé**  

- Lorsque le processus enfant se termine, il transmet son état au parent via une structure appelée « entrée dans la table des processus ».  
- Si le parent ne récupère pas cet état avec `wait()`, l'entrée reste active et devient un zombie.

---

### **Exemple concret**  

Imaginez un programme (parent) qui lance un calcul (enfant). Si le parent ne « nettoie » pas le résultat du calcul, le système conserve une trace inutile du processus enfant, ce qui encombre la table des processus.

---

## 2. Expérimentation guidée

### Préparation

1. **Ouvrir Terminator**  
   Divisez votre terminal en deux colonnes (cliquez droit → « Split vertically ») pour avoir une fenêtre gauche et une fenêtre droite.

2. **Lancer le programme**  
   Dans le terminal gauche, exécutez le programme qui simule des processus zombies :  
   ```bash
   proczombi
   ```

   Laissez ce terminal actif pour interagir avec le menu.

---

### Étape 1 : Identifier un processus zombie

1. Dans le **terminal droit**, affichez la liste des processus en cours d’exécution à l’aide de la commande `ps` :  
   ```bash
   ps -ef | grep proczombi
   ```

   #### **Explications :**
   - `ps` : Commande qui affiche une liste des processus actifs.  
   - `-e` : Montre tous les processus en cours sur le système.  
   - `-f` : Affiche des informations détaillées, comme les PID, les PPID et les noms des processus.  
   - `grep proczombi` : Filtre les résultats pour ne montrer que ceux contenant le mot « proczombi ».

   **Indice :** Repérez les **PID** (identifiants) des processus parent et enfant.

2. Revenez au terminal gauche, utilisez le menu du programme pour obtenir des informations supplémentaires sur les processus parent et enfant. Notez les PID pour la suite.

3. Revenez au terminal droit. Tuez le processus enfant :  
   ```bash
   kill -9 <PID_enfant>
   ```

   #### **Explications :**
   - `kill` : Envoie un signal à un processus pour le contrôler (exemple : le terminer).  
   - `-9` : Signal SIGKILL, qui force la terminaison immédiate du processus.  
   - `<PID_enfant>` : Remplacez `<PID_enfant>` par l’identifiant du processus enfant.

4. Vérifiez son état avec la commande suivante :  
   ```bash
   ps -l
   ```

   #### **Explications :**
   - `-l` : Mode long qui affiche des informations supplémentaires, comme le statut (*state*) du processus.  
   - Regardez la colonne `STAT`.  

   **Indice :** Un zombie est signalé par un `Z` dans la colonne `STAT`.

---

#### **Question d’observation**  
- Que remarquez-vous dans la colonne `STAT` pour l’enfant après l’avoir tué ?  
  *(Indice : Le processus est toujours visible, mais il est marqué comme un zombie.)*  

---

### Étape 2 : Nettoyer un zombie

1. Revenez dans le terminal gauche. Utilisez l’option du menu permettant d’exécuter la commande `wait()`.  
2. Revenez au terminal droit et vérifiez que le zombie a disparu :  
   ```bash
   ps -l
   ```

---

#### **Question d’analyse**  
- Pourquoi l’utilisation de `wait()` a-t-elle supprimé le zombie ?  
  *(Indice : `wait()` permet au parent de récupérer l’état du processus enfant, ce qui nettoie son entrée dans la table des processus.)*

---

## 3. Exercices pratiques

### Exercice 1 : Comprendre la commande `ps`

Dans le terminal droit, exécutez les commandes suivantes et comparez leurs résultats :  

1. Liste complète des processus avec `ps` :  
   ```bash
   ps -e
   ```

2. Affichage détaillé :  
   ```bash
   ps -ef
   ```

3. Affichage en mode long :  
   ```bash
   ps -l
   ```

#### Questions :
- Quelle commande montre les relations entre les processus (PID et PPID) ?  
- Quelle commande affiche le statut (`STAT`) des processus ?  

---

### Exercice 2 : Expérimenter avec plusieurs zombies

1. Relancez `proczombi`.  
2. Modifiez le menu pour créer plusieurs enfants.  
3. Tuez un ou plusieurs enfants et observez leurs états.  

#### Questions :  
- Que se passe-t-il si le parent n’appelle pas `wait()` immédiatement ?  
- Comment vérifier l’état des processus zombies ?  

---

### Exercice 3 : Réflexion critique

1. Expliquez en une phrase pourquoi les zombies consomment peu de ressources mais doivent être évités.  
2. Quels problèmes pourraient survenir si un programme génère de nombreux zombies ?  

---

## 4. Résumé des points clés

- Un **processus zombie** se produit lorsqu’un enfant termine son exécution, mais que son parent n’a pas récupéré son état avec `wait()`.  
- Un zombie est identifiable grâce à la colonne `STAT` avec la valeur `Z`.  
- Les zombies consomment peu de ressources, mais encombrent la table des processus, ce qui peut nuire à la stabilité du système si leur nombre devient élevé.  
- Utilisez `wait()` dans le parent pour récupérer l’état d’un enfant et supprimer les zombies.

---

## 5. Évaluation des connaissances

### **Questions à choix multiples**

**Q1 : Qu’est-ce qui fait qu’un processus devient un zombie ?**  
1. Il tourne indéfiniment.  
2. Il est mort, mais son parent n’a pas récupéré son état.  
3. Il n’a jamais été lancé correctement.  

> **Réponse attendue :** 2.  

---

**Q2 : Quels outils permettent d’observer les processus zombies sous Linux ?**  
1. `htop`  
2. `ps`  
3. `wait`  
4. `kill`  

> **Réponses attendues :** 1 et 2.  

---

**Q3 : Que se passe-t-il si un parent meurt avant d’appeler `wait()` sur ses enfants ?**  
1. Les enfants deviennent orphelins.  
2. Les enfants deviennent zombies.  
3. `init` ou `systemd` adoptent les enfants.  

> **Réponse attendue :** 1 et 3.

---

## 6. Lien avec les notions futures

- **Processus orphelins** : Dans une prochaine activité, vous apprendrez ce qu’il advient des processus lorsqu’un parent meurt avant eux.  
- **Gestion des ressources système** : Comment les systèmes d’exploitation gèrent efficacement des milliers de processus actifs.  

---

### **Notes pédagogiques**  
Cette activité guide les étudiants dans la découverte des processus zombies tout en leur permettant de se familiariser avec des commandes essentielles comme `ps`, `kill` et `wait()`. Les questions et indices encouragent une réflexion active. Vous pouvez enrichir l’activité avec des schémas pour visualiser les relations entre parent, enfant et système.  
