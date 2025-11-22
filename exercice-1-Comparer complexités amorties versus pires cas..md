Bonjour !

Ce TP vous propose d'explorer une notion fondamentale en algorithmique : la complexité amortie. Nous allons l'illustrer à travers un cas d'étude classique et très concret : l'insertion dans une table de hachage qui se redimensionne.

---

## TP : Complexité Amortie de l'Insertion dans une Table de Hachage Dynamique

### Objectif du TP

Comparer la complexité amortie et la complexité pire cas de l'opération d'insertion dans une structure de données dynamique.

### Contexte

Les tables de hachage sont des structures de données très efficaces pour le stockage et la récupération d'éléments, offrant en moyenne des opérations en temps constant (O(1)). Cependant, leur performance dépend fortement du "facteur de charge" (nombre d'éléments / taille du tableau sous-jacent). Si ce facteur devient trop élevé, les collisions augmentent, dégradant les performances. Pour maintenir une efficacité optimale, les tables de hachage sont souvent conçues pour se redimensionner : lorsque le facteur de charge dépasse un certain seuil, la table alloue un tableau plus grand (souvent le double de la taille actuelle) et re-hache tous les éléments existants dans la nouvelle structure. Cette opération de redimensionnement est coûteuse, potentiellement en O(N) où N est le nombre d'éléments.

Ce coût ponctuel élevé soulève une question : si une insertion peut coûter O(N) dans le pire des cas (lors d'un redimensionnement), comment peut-on affirmer que l'insertion dans une table de hachage est en moyenne O(1) ? C'est là qu'intervient la notion de complexité amortie.

### Travail Préparatoire

Avant de vous lancer dans le code, prenez un moment pour structurer votre approche.

1.  **Représentation :** Comment allez-vous représenter votre table de hachage ? Un tableau de listes chaînées est une approche simple et efficace pour gérer les collisions (hachage par chaînage).
2.  **Fonction de hachage :** Quelle fonction de hachage simple utiliserez-vous pour des clés entières ou des chaînes de caractères ? (Ex: `clé % taille_tableau`).
3.  **Déclenchement du redimensionnement :** À quel moment précis le redimensionnement doit-il avoir lieu ? Quel seuil de facteur de charge (par exemple, 0.7 ou 0.75) vous semble pertinent ?
4.  **Stratégie de redimensionnement :** Quelle sera la nouvelle taille du tableau ? (Ex: doubler la taille).

N'hésitez pas à solliciter une IA pour générer une structure de base de classe `HashTable` ou des fonctions utilitaires comme une fonction de hachage simple. L'objectif est de vous concentrer sur la logique de redimensionnement et l'analyse de complexité.

### Partie 1 : Implémentation de la Table de Hachage Dynamique

Implémentez une classe `HashTable` dans le langage de votre choix (Python, Java, C++, etc.) avec les fonctionnalités suivantes :

1.  **Constructeur :** Initialise la table avec une taille de tableau initiale (par exemple, 10 ou 16) et un seuil de facteur de charge.
2.  **`insert(key, value)` :**
    *   Calcule l'indice de hachage pour la clé.
    *   Insère la paire (clé, valeur) dans la liste chaînée correspondante.
    *   **Mesure du coût :** Cette fonction doit retourner le "coût" de l'opération d'insertion. Le coût peut être défini comme le nombre d'opérations élémentaires effectuées :
        *   Calcul du hachage.
        *   Parcours de la liste chaînée (nombre de comparaisons de clés).
        *   Opérations d'affectation.
        *   **Si un redimensionnement est déclenché :** Incluez dans ce coût toutes les opérations liées au redimensionnement (création du nouveau tableau, re-hachage et réinsertion de *tous* les éléments existants).
    *   Après l'insertion, vérifie si le facteur de charge dépasse le seuil. Si oui, appelle une méthode de redimensionnement interne.
3.  **`_resize()` (méthode interne) :**
    *   Crée un nouveau tableau de taille augmentée (par exemple, `ancienne_taille * 2`).
    *   Parcourt tous les éléments de l'ancien tableau.
    *   Pour chaque élément, le re-hache et l'insère dans le nouveau tableau.
    *   Met à jour la référence du tableau interne de la `HashTable`.
    *   **Mesure du coût :** Cette méthode doit accumuler le coût de toutes les opérations de re-hachage et de réinsertion.

### Partie 2 : Expérimentation et Collecte de Données

Écrivez un script pour tester votre implémentation :

1.  Créez une instance de votre `HashTable`.
2.  Insérez un grand nombre d'éléments (par exemple, 100 000 à 1 000 000) avec des clés aléatoires (par exemple, des entiers uniques générés aléatoirement).
3.  Pour *chaque* insertion, enregistrez le coût retourné par la méthode `insert()`.
4.  Stockez ces coûts dans une liste ou un tableau.
5.  **Visualisation :**
    *   Tracez un graphique montrant le coût de chaque insertion en fonction du nombre total d'insertions effectuées (axe X : nombre d'insertions, axe Y : coût de l'insertion N).
    *   Calculez et affichez le coût moyen par insertion sur l'ensemble des opérations.

### Partie 3 : Analyse et Discussion

Rédigez un court rapport (quelques paragraphes) répondant aux questions suivantes :

1.  **Observation du graphique :** Décrivez ce que vous observez sur le graphique des coûts d'insertion. Identifiez clairement les "pics" et les périodes de faible coût.
2.  **Complexité pire cas :** Expliquez pourquoi le coût d'une *seule* insertion peut être très élevé (O(N)) dans le pire des cas, en vous basant sur vos observations et le mécanisme de redimensionnement.
3.  **Complexité amortie :** Expliquez pourquoi, malgré ces pics, la complexité amortie de l'insertion est considérée comme O(1). Utilisez l'intuition de la "méthode des crédits" ou de la "méthode du potentiel" : comment les opérations peu coûteuses "paient" pour les opérations coûteuses de redimensionnement ?
4.  **Impact des paramètres :** Comment le choix du seuil de facteur de charge et du facteur de redimensionnement (par exemple, doubler la taille vs. tripler la taille) pourrait-il influencer le nombre et l'amplitude des pics, ainsi que le coût amorti ?
5.  **Réflexion sur l'IA :** Comment l'IA vous a-t-elle aidé dans ce processus (génération de code, explication de concepts, débogage) ? Où avez-vous dû intervenir pour corriger, affiner ou valider son travail ?

### Rendu

Votre rendu devra inclure :

*   Le code source de votre implémentation de `HashTable` et de votre script d'expérimentation.
*   Le(s) graphique(s) généré(s).
*   Votre rapport d'analyse et de discussion.

Bon courage !
