Bonjour à toutes et à tous,

Ce TP vous invite à explorer un aspect crucial de la sécurité et de la robustesse des systèmes basés sur l'apprentissage automatique : les attaques adversariales. Nous allons nous concentrer sur un type spécifique d'index, les "learned indexes", qui utilisent des modèles d'apprentissage pour optimiser les recherches de données.

---

## Sujet de TP : Attaque Adversariale Simple sur un Index Appris Linéaire

### Objectif du TP

Analyser un scénario d'attaque adversariale sur un learned index en modélisant et simulant une attaque simple pour en évaluer l'impact sur les performances.

### Énoncé du TP

Modéliser et simuler une attaque adversariale simple sur un learned index, en mesurant la dégradation des performances de recherche.

---

### Contexte et Rappels

Les "learned indexes" remplacent les structures d'indexation traditionnelles (comme les B-trees ou les hash tables) par des modèles d'apprentissage automatique. L'idée est que, pour des données triées, un modèle peut apprendre la fonction de distribution cumulative (CDF) des clés et prédire la position approximative d'une clé dans le dataset. Une recherche se déroule généralement en deux étapes :
1.  **Prédiction du modèle :** Le modèle prédit une position `p_pred` pour une clé `k`.
2.  **Correction locale :** Une petite recherche linéaire ou une structure auxiliaire (comme un mini B-tree) est utilisée autour de `p_pred` pour trouver la position exacte de `k`. La taille de cette zone de correction dépend de l'erreur maximale du modèle.

Une attaque adversariale vise à perturber légèrement une entrée (ici, une clé de recherche) de manière à ce que le modèle produise une prédiction erronée, augmentant ainsi le coût de la phase de correction locale et dégradant les performances globales de l'index.

### Mini-Projet : Attaque sur un Index Linéaire Appris

Nous allons travailler avec un scénario simplifié : un index appris basé sur une régression linéaire.

#### 1. Préparation de l'Environnement

*   Utilisez Python. Les bibliothèques `numpy` pour la manipulation de données, `scikit-learn` pour la régression linéaire et `matplotlib` pour la visualisation seront utiles.
*   Assurez-vous que votre environnement est prêt à exécuter ces scripts.

#### 2. Modélisation de l'Index Appris

1.  **Génération de Données :**
    *   Créez un dataset synthétique de `N` clés numériques triées. Par exemple, `N=10000`.
    *   Les clés peuvent être des entiers simples (`[0, 1, 2, ..., N-1]`) ou des valeurs plus complexes mais toujours triées (ex: `[10, 20, 30, ..., 10*N]`).
    *   Pour chaque clé `k`, sa "vraie" position `pos(k)` est simplement son index dans le tableau trié.
2.  **Entraînement du Modèle :**
    *   Entraînez un modèle de régression linéaire simple (`sklearn.linear_model.LinearRegression`) pour apprendre la fonction `f(clé) = position`.
    *   Le modèle prendra les clés en entrée et les positions correspondantes en sortie.
3.  **Évaluation de l'Index :**
    *   Calculez l'erreur maximale (`max_error`) et l'erreur moyenne absolue (`MAE`) de votre modèle sur le dataset d'entraînement.
    *   Définissez une fonction de recherche (`lookup(key)`) qui simule le processus :
        *   `p_pred = model.predict(key)`
        *   La recherche locale est simulée par un balayage linéaire autour de `p_pred` dans une fenêtre de taille `2 * max_error` (ou une autre heuristique que vous définirez, par exemple `2 * MAE + C`).
        *   Le "coût" d'une recherche sera le nombre d'étapes nécessaires pour trouver la clé exacte après la prédiction du modèle. Pour simplifier, vous pouvez définir ce coût comme `|p_pred - pos(key)| + C` (où `C` est une constante pour la recherche dans la fenêtre d'erreur minimale).

#### 3. Conception de l'Attaque Adversariale Simple

L'objectif est de trouver une petite perturbation `delta` pour une clé `k_cible` telle que la prédiction du modèle pour `k_cible + delta` soit significativement éloignée de la vraie position de `k_cible`.

1.  **Choix d'une Clé Cible :** Sélectionnez une clé `k_cible` dans votre dataset.
2.  **Définition de la Perturbation :**
    *   Une perturbation `delta` est un petit ajout ou soustraction à la clé originale.
    *   Fixez une contrainte sur la magnitude de `delta`, par exemple `|delta| <= epsilon`, où `epsilon` est une petite valeur (ex: 0.1, 0.5, 1.0, selon la plage de vos clés).
3.  **Stratégie d'Attaque (FGSM-like) :**
    *   Pour un modèle linéaire `f(x) = ax + b`, la prédiction est `p_pred = ax + b`.
    *   L'erreur que nous voulons maximiser est `|f(k_cible + delta) - pos(k_cible)|`.
    *   Pour maximiser cette erreur avec un `delta` de magnitude `epsilon`, nous pouvons utiliser une approche inspirée de FGSM (Fast Gradient Sign Method) :
        *   Calculez la "direction" de l'erreur : `sign(f(k_cible) - pos(k_cible))`.
        *   Appliquez `delta = epsilon * sign(a)` si vous voulez augmenter la prédiction, ou `delta = -epsilon * sign(a)` si vous voulez la diminuer.
        *   Plus simplement, pour un modèle linéaire `f(x) = ax+b`, nous savons que `f(x+delta) = f(x) + a*delta`.
        *   Pour maximiser `|f(k_cible) + a*delta - pos(k_cible)|`, choisissez `delta = epsilon` si `a` et `(f(k_cible) - pos(k_cible))` ont le même signe, ou `delta = -epsilon` s'ils ont des signes opposés.
        *   *Implémentation simplifiée :* Testez `k_attaquée_1 = k_cible + epsilon` et `k_attaquée_2 = k_cible - epsilon`. Choisissez celle qui maximise `|model.predict(k_attaquée) - pos(k_cible)|`.
4.  **Clé Attaquée :** Créez la clé `k_attaquée = k_cible + delta_optimal`.

#### 4. Simulation et Analyse

1.  **Mesure de la Dégradation :**
    *   Calculez le coût de recherche pour `k_cible` (non attaquée).
    *   Calculez le coût de recherche pour `k_attaquée`.
    *   Comparez les deux coûts.
2.  **Visualisation :**
    *   Tracez la fonction `f(clé)` de votre modèle.
    *   Marquez la position de `k_cible` et `pos(k_cible)`.
    *   Marquez la position de `k_attaquée` et la prédiction `f(k_attaquée)`.
    *   Visualisez la "distance" entre `f(k_attaquée)` et `pos(k_cible)`.
3.  **Analyse :**
    *   Décrivez l'impact de l'attaque sur le coût de recherche.
    *   Expliquez pourquoi cette attaque fonctionne sur un index linéaire appris.
    *   Discutez des limites de cette attaque simple.
    *   Proposez des pistes pour des attaques plus sophistiquées ou des contre-mesures potentielles (sans les implémenter).

---

### Consignes pour la Rédaction et la Présentation

Votre rendu devra inclure :

1.  **Le code Python** complet et commenté, permettant de reproduire votre simulation.
2.  **Un rapport concis** (quelques paragraphes) qui présente :
    *   Les résultats de votre simulation (coûts de recherche avant/après attaque).
    *   Les visualisations pertinentes.
    *   Votre analyse de l'efficacité de l'attaque et des raisons de cette efficacité.
    *   Une discussion sur les limites de cette attaque simple et des idées de contre-mesures.

**Note sur l'utilisation de l'IA :** N'hésitez pas à utiliser des outils d'IA générative (comme ChatGPT, Copilot, etc.) pour vous aider dans la rédaction du code, la compréhension des concepts ou la structuration de votre rapport. L'objectif est d'apprendre et de comprendre, pas de réinventer la roue. Cependant, assurez-vous de bien comprendre le code généré, de le tester, de le déboguer et de l'adapter à vos besoins. La capacité à critiquer et à valider les propositions de l'IA est une compétence clé. Votre analyse et votre compréhension des résultats restent au cœur de l'évaluation.

Bon courage pour ce TP !