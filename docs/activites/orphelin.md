# Activité pédagogique : Comprendre les processus orphelins

---

## Objectif pédagogique

Apprendre à identifier, comprendre et manipuler un **processus orphelin** (*orphan process*) sous Linux par l’expérimentation.  
Cette activité permet également de découvrir et d'utiliser des commandes essentielles pour la gestion des processus.

---

## 1. Introduction : Qu’est-ce qu’un processus orphelin ?

### **Définition**

Un **processus orphelin** (*orphan process*) est un processus dont le **processus parent** est mort avant qu'il ne termine son exécution.  
Sous Linux, lorsqu'un processus parent meurt, ses enfants sont automatiquement adoptés par le processus système **`init`** (ou `systemd` selon la distribution), qui a toujours pour PID `1`.

> **ℹ️ Qu’est-ce que `init` et `systemd` ?**  
> - **`init`** et **`systemd`** sont des programmes essentiels du système Linux, appelés **gestionnaires de démarrage** (*init systems*).  
> - Leur rôle principal est de **superviser les processus** dès le démarrage du système et de maintenir le fonctionnement global.  
> - Si un processus perd son parent, `init` ou `systemd` en devient le nouveau parent pour garantir qu’aucun processus ne reste sans supervision.  
> - Certaines distributions Linux utilisent **`init`** (comme Debian dans ses anciennes versions) tandis que d'autres utilisent **`systemd`** (par exemple, Ubuntu et les versions récentes de Debian).  
> - **Analogie** : Pensez à `init` ou `systemd` comme une "personne de garde" qui prend en charge les enfants laissés sans parent. Peu importe qui est en service (`init` ou `systemd`), ils assument la responsabilité.  

Cette adoption automatique empêche les processus orphelins de devenir problématiques pour le système.  

---

### **Concept clé**  

- Un processus orphelin n'est pas problématique car il est pris en charge par `init`.
- Contrairement aux zombies, les orphelins ne consomment pas inutilement de ressources, car ils continuent à être gérés par le système.

---

### **Exemple concret**  

Imaginez un programme (parent) qui effectue une tâche, comme surveiller un téléchargement (enfant). Si le parent est tué avant que l'enfant ne termine, le système adopte l'enfant pour qu'il puisse continuer son exécution.

---

## 2. Expérimentation guidée

### Préparation

1. **Ouvrir Terminator**  
   Divisez votre terminal en deux colonnes (cliquez droit → « Split vertically ») pour avoir une fenêtre gauche et une fenêtre droite.

2. **Lancer le programme**  
   Dans le terminal gauche, exécutez le programme de démonstration qui simule les processus orphelins :  
   ```bash
   procorphan
   ```

   Laissez ce terminal actif pour interagir avec le menu.

---

### Étape 1 : Observer les processus parent et enfant

1. Dans le **terminal droit**, affichez la liste des processus en cours d’exécution :  
    ```bash
    ps -ef | grep procorphan
    ```

    **Explications :**

    - `ps` : Commande qui affiche une liste des processus actifs.  
    - `-e` : Montre tous les processus en cours sur le système.  
    - `-f` : Affiche des informations détaillées, comme les PID, les PPID et les noms des processus.  
    - `grep procorphan` : Filtre les résultats pour ne montrer que ceux contenant le mot « procorphan ».

    **Indice :** Identifiez les **PID** (identifiants) des processus parent et enfant et notez-les.

2. Revenez au terminal gauche. Utilisez le menu pour confirmer les informations sur les processus parent et enfant.

---

### Étape 2 : Provoquer un processus orphelin

1. Revenez au terminal droit et tuez le processus parent :  
   ```bash
   kill -9 <PID_parent>
   ```

   #### **Explications :**
   - `kill` : Envoie un signal à un processus pour le contrôler (exemple : le terminer).  
   - `-9` : Signal SIGKILL, qui force la terminaison immédiate du processus.  
   - `<PID_parent>` : Remplacez `<PID_parent>` par l’identifiant du processus parent.

2. Affichez à nouveau les processus pour observer les changements :  
   ```bash
   ps -ef | grep procorphan
   ```

   **Indice :** Vérifiez le **PPID** (Parent Process ID) du processus enfant. Si l’enfant est orphelin, son **PPID** sera `1` (le PID de `init` ou `systemd`).

---

#### **Question d’observation**  
- Que remarquez-vous dans la colonne **PPID** du processus enfant après la mort du parent ?  
  *(Indice : Le PPID indique que l’enfant est désormais adopté par `init`.)*  

---

### Étape 3 : Continuité d’un processus orphelin

1. Observez dans le terminal gauche que l’enfant continue son exécution même après la mort du parent.  
2. Revenez au terminal droit et confirmez qu’un processus orphelin est pris en charge par `init`.  

---

#### **Question d’analyse**  
- Pourquoi le système adopte-t-il un processus orphelin ?  
  *(Indice : Cela garantit que tous les processus en cours sont supervisés.)*

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

3. Filtrage des processus liés à `procorphan` :  
   ```bash
   ps -ef | grep procorphan
   ```

#### Questions :
- Quelle commande montre les relations entre les processus (PID et PPID) ?  
- Quelle commande permet d’afficher uniquement les processus liés à un programme spécifique ?  

---

### Exercice 2 : Expérimenter avec plusieurs enfants

1. Relancez `procorphan`.  
2. Modifiez le menu pour créer plusieurs enfants.  
3. Tuez le parent et observez les **PPID** des enfants.  

#### Questions :  
- Que se passe-t-il pour les enfants après la mort du parent ?  
- Comment vérifier qu’ils sont maintenant adoptés par `init` ?  

---

### Exercice 3 : Réflexion critique

1. Expliquez pourquoi les processus orphelins ne posent pas de problème au système.  
2. Quelle différence fondamentale existe-t-il entre un zombie et un orphelin ?  

---

## 4. Résumé des points clés

- Un **processus orphelin** survient lorsqu’un parent meurt avant son enfant.  
- Sous Linux, les orphelins sont automatiquement adoptés par `init` (PID = 1).  
- Les processus orphelins continuent à fonctionner normalement et ne consomment pas de ressources inutiles.  
- Utilisez `ps` pour vérifier le **PPID** des processus et observer les relations entre parent et enfant.

---

## 5. Évaluation des connaissances

### **Questions à choix multiples**

**Q1 : Que devient un processus enfant lorsque son parent meurt ?**  
1. Il est adopté par `init`.  
2. Il est arrêté immédiatement.  
3. Il devient un zombie.  

> **Réponse attendue :** 1.  

---

**Q2 : Quels outils permettent d’observer les processus orphelins sous Linux ?**  
1. `htop`  
2. `ps`  
3. `kill`  
4. `grep`  

> **Réponses attendues :** 1, 2 et 4.  

---

**Q3 : Quelle est la différence entre un zombie et un orphelin ?**  
1. Un zombie a encore son parent actif.  
2. Un zombie n’est pas adopté par `init`.  
3. Un orphelin consomme des ressources inutilement.  

> **Réponse attendue :** 1 et 2.

---

## 6. Lien avec les notions futures

- **Processus zombies** : Comparez avec les processus zombies étudiés dans une activité précédente.  
- **Gestion des processus par le système** : Étudiez comment `init` et `systemd` supervisent les processus pour assurer la stabilité du système.

---

### **Notes pédagogiques**  
Cette activité permet aux étudiants d’expérimenter les processus orphelins tout en découvrant des commandes essentielles (`ps`, `kill`, `grep`). Les questions et indices guident l’apprentissage actif et favorisent une réflexion approfondie. Vous pouvez enrichir l’activité avec des diagrammes pour visualiser les relations parent-enfant dans les processus Linux.  

