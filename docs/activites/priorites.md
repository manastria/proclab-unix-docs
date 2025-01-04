# Scénario : Processus orphelins



Voici un programme en C, nommé **`procprio`**, qui permet d’expérimenter la gestion des priorités sous Linux avec les commandes `nice` et `renice`. Le programme exécute une tâche CPU-intensive (par exemple, un calcul simple dans une boucle infinie) pour illustrer comment les priorités influencent son comportement.


### Fonctionnement du programme

1. **Objectif du programme :**
   - Simuler un processus qui effectue une tâche CPU-intensive.
   - Afficher régulièrement le PID, la priorité (via la commande `getpriority`), et un compteur.

2. **Options du programme :**
   - Le programme accepte un argument optionnel pour définir la priorité initiale via `nice` :
     ```bash
     ./procprio <valeur_nice>
     ```

3. **Exécution sans argument :**
   - La priorité par défaut (`nice = 0`) est utilisée.

---

### Instructions pédagogiques pour le scénario

#### **Étape 1 : Lancer plusieurs instances de `procprio`**

1. Ouvrez **Terminator** et divisez le terminal en plusieurs colonnes.
2. Lancez plusieurs instances du programme avec différentes valeurs de `nice` :
   - Exemple :
     ```bash
     ./procprio 0
     ./procprio 10
     ./procprio 19
     ```
3. Observez les processus dans un autre terminal avec `htop` ou `ps` :
   ```bash
   htop
   ```
   - Utilisez la touche `F3` dans `htop` pour rechercher les processus `procprio`.
   - Regardez la colonne `NI` pour voir les priorités.

---

#### **Étape 2 : Modifier les priorités avec `renice`**

1. Trouvez le PID des processus `procprio` :
   ```bash
   ps -ef | grep procprio
   ```

2. Ajustez les priorités avec la commande `renice` :
   - Exemple pour réduire la priorité d’un processus :
     ```bash
     sudo renice 5 -p <PID>
     ```
   - Exemple pour augmenter la priorité d’un processus (nécessite `sudo`) :
     ```bash
     sudo renice -10 -p <PID>
     ```

3. Revenez à `htop` ou `ps` pour vérifier les changements dans la colonne `NI`.

---

#### **Étape 3 : Observer l’impact des priorités**

1. Surveillez les sorties des différentes instances de `procprio`.
   - Les processus avec une priorité plus élevée (valeur `nice` plus basse) devraient progresser plus rapidement.
   - Les processus avec une priorité plus basse (valeur `nice` plus haute) devraient ralentir.

2. Analysez les ressources CPU utilisées par chaque processus dans `htop`.
   - La colonne `%CPU` indique l’utilisation du processeur.

---

### Exercices pratiques

1. **Expérience avec `nice` :**
   - Lancez deux instances de `procprio` :
     ```bash
     ./procprio 0
     ./procprio 19
     ```
   - Observez leurs performances respectives dans `htop`.

2. **Expérience avec `renice` :**
   - Lancez une instance de `procprio` avec une priorité élevée (`nice = -5`).
   - Pendant son exécution, changez sa priorité avec `renice` pour la diminuer.
     - Exemple :
       ```bash
       sudo renice 10 -p <PID>
       ```
   - Observez comment cela affecte son utilisation CPU.

3. **Analyse comparative :**
   - Comparez l’impact des priorités sur des processus utilisant intensivement le CPU.
   - Déterminez comment `nice` et `renice` influencent les performances globales du système.

---

### Tableau récapitulatif des commandes

| **Commande**                   | **Explication**                                                                                          |
|--------------------------------|----------------------------------------------------------------------------------------------------------|
| `nice <valeur> <commande>`     | Lance une commande avec une priorité définie. La valeur `nice` va de -20 (priorité haute) à 19 (priorité basse). |
| `renice <valeur> -p <PID>`     | Modifie la priorité d’un processus en cours. Nécessite `sudo` pour diminuer la valeur `nice`.            |
| `ps -o pid,ni,cmd`             | Affiche les priorités (`NI`) des processus en cours.                                                     |
| `htop`                         | Affiche un moniteur interactif des processus, avec la colonne `NI` pour les priorités.                  |

---

### Questions de réflexion

1. **Observation des priorités :**
   - Comment évoluent les performances d’un processus lorsque sa valeur `nice` est augmentée ?
   - Pourquoi une priorité plus basse (valeur `nice` plus élevée) diminue-t-elle l’utilisation CPU d’un processus ?

2. **Expérience avec `renice` :**
   - Comment les changements de priorité influencent-ils immédiatement un processus ?
   - Pourquoi faut-il des privilèges administratifs pour diminuer une valeur `nice` (augmenter la priorité) ?

3. **Impact sur les performances globales :**
   - Que se passe-t-il si plusieurs processus ont des priorités similaires et utilisent intensivement le CPU ?
   - Dans quels scénarios réels utiliseriez-vous `nice` ou `renice` pour optimiser un système ?

---

### Résumé des points clés

- La priorité d’un processus détermine son accès au CPU. Une priorité plus élevée (valeur `nice` plus basse) lui accorde plus de temps processeur.
- `nice` est utilisé pour définir la priorité d’un processus au démarrage, tandis que `renice` permet de la modifier après le démarrage.
- Des outils comme `htop` permettent de visualiser facilement l’impact des priorités sur les processus en cours.

---

### Évaluation des connaissances

1. **Que signifie une valeur `nice` élevée (par exemple, 19) ?**
   1. Priorité haute.
   2. Priorité basse.
   3. Priorité neutre.

   > **Réponse attendue :** 2.

2. **Pourquoi faut-il utiliser `sudo` pour diminuer une valeur `nice` ?**
   1. Cela réduit les privilèges du processus.
   2. Cela augmente la priorité du processus, ce qui peut affecter le système globalement.
   3. Cela ne nécessite pas `sudo`.

   > **Réponse attendue :** 2.

3. **Quel outil interactif affiche la colonne `NI` pour visualiser les priorités ?**
   1. `ps`
   2. `htop`
   3. `nice`

   > **Réponse attendue :** 2.
