VOTRE NOM :
VOTRE PRENOM :


# Examen d’Algorithmique Avancée et Complexité

**Durée : 1h30**
**IA : Non autorisée**


## Structure de l’examen

* **Partie A – QCM (40 questions)** 
* **Partie B – Analyse de code et complexité** 
* **Partie C – Mini-projet guidé** 

---

# PARTIE A – QCM

> **Consigne** : Une seule bonne réponse par question.

### I. Complexité et concepts généraux (Questions 1 à 10)

1. La notation Θ(f(n)) exprime :
   A. Une borne supérieure
   B. Une borne inférieure
   C. Une approximation expérimentale
   D. Une borne asymptotiquement serrée

   Votre réponse :

2. La complexité en pire cas de la recherche dans un tableau non trié est :
   A. O(1)
   B. O(log n)
   C. O(n)
   D. O(n log n)

   Votre réponse :

3. Quel cas de complexité est le plus pertinent pour garantir une performance minimale ?
   A. Meilleur cas
   B. Cas moyen
   C. Pire cas
   D. Cas amorti

   Votre réponse :   

4. Une analyse amortie permet principalement de :
   A. Lisser le coût de plusieurs opérations
   B. Calculer le pire cas exact
   C. Optimiser la mémoire
   D. Éviter la récursivité

   Votre réponse :

5. Quelle structure utilise naturellement la pile d’exécution en C ?
   A. File
   B. Tas
   C. Pile
   D. Liste chaînée

   Votre réponse :

6. L’utilisation incorrecte de `free` peut entraîner :
   A. Une fuite mémoire
   B. Un dangling pointer
   C. Un segmentation fault
   D. Toutes les réponses précédentes

   Votre réponse :

7. Quelle opération est en O(1) dans une liste doublement chaînée (avec pointeur sur le nœud) ?
   A. Recherche
   B. Suppression
   C. Parcours
   D. Tri

   Votre réponse :

8. Une table de hachage bien dimensionnée offre en moyenne une recherche en :
   A. O(1)
   B. O(log n)
   C. O(n)
   D. O(n log n)

   Votre réponse :

9. Dans une table de hachage à adressage ouvert, les collisions sont gérées par :
   A. Des listes chaînées
   B. Des arbres
   C. Une exploration de cases alternatives
   D. Une réallocation mémoire

   Votre réponse :

10. Une fonction de hachage doit idéalement :
    A. Être simple uniquement
    B. Être cryptographiquement sûre
    C. Être bien aiguisée
    D. Répartir uniformément les clés

   Votre réponse :


### II. Structures de données et arbres (Questions 11 à 20)

11. Un ABR dégénéré a une complexité de recherche en :
    A. O(n)
    B. O(log n)
    C. O(1)
    D. O(n log n)

   Votre réponse :

12. Le parcours infixe d’un ABR affiche les clés :
    A. Dans l’ordre inverse
    B. Dans l’ordre d’insertion
    C. Dans l’ordre croissant
    D. De manière aléatoire

   Votre réponse :

13. Le principal objectif d’un arbre équilibré est de :
    A. Garantir une hauteur logarithmique
    B. Réduire la mémoire
    C. Faciliter l’implémentation
    D. Supprimer la récursivité

   Votre réponse :

14. Une rotation dans un arbre AVL sert à :
    A. Supprimer un nœud
    B. Rééquilibrer l’arbre
    C. Trier les clés
    D. Compresser l’arbre

   Votre réponse :

15. Un arbre Rouge-Noir garantit une hauteur maximale de :
    A. log₂(n)
    B. n/2
    C. n
    D. 2 log₂(n)

   Votre réponse :

16. Quelle structure est la plus adaptée pour implémenter BFS ?
    A. Une pile
    B. Une file
    C. Un Tas
    D. Liste circulaire

   Votre réponse :

17. DFS utilise naturellement :
    A. Une file
    B. Un tas
    C. Une pile (explicite ou récursive)
    D. Une table de hachage

   Votre réponse :

18. La représentation par listes d’adjacence est plus efficace pour :
    A. Graphes creux
    B. Graphes denses
    C. Graphes complets uniquement
    D. Tous les graphes

   Votre réponse :

19. La complexité de BFS avec listes d’adjacence est :
    A. O(V)
    B. O(E)
    C. O(V log E)
    D. O(V + E)

   Votre réponse :

20. Un graphe orienté peut contenir :
    A. Un seul chemin
    B. Un arbre uniquement
    C. Un nombre pair de sommets
    D. Des cycles

   Votre réponse :



### III. Paradigmes algorithmiques et programmation dynamique (Questions 21 à 40)

21. Divide & Conquer repose sur :
    A. Itération simple
    B. Diviser, résoudre, combiner
    C. Programmation dynamique
    D. Recherche exhaustive

   Votre réponse :

22. Merge Sort a une complexité garantie de :
    A. O(n²)
    B. O(n log n)
    C. O(log n)
    D. O(n)

   Votre réponse :

23. Le pire cas de Quick Sort est :
    A. O(n log n)
    B. O(log n)
    C. O(n²)
    D. O(n)

   Votre réponse :

24. Le choix du pivot influence :
    A. La complexité
    B. La stabilité
    C. La mémoire uniquement
    D. Le type de données

   Votre réponse :

25. Le backtracking est adapté aux problèmes :
    A. Linéaires
    B. Gloutons
    C. Combinatoires
    D. Numériques simples

   Votre réponse :

26. Le problème des N-Dames est résolu classiquement par :
    A. Programmation dynamique
    B. Algorithme glouton
    C. Divide & Conquer
    D. Backtracking

   Votre réponse :

27. Une solution par backtracking explore :
    A. Un seul chemin
    B. Tous les chemins possibles avec retour arrière
    C. Un graphe pondéré
    D. Un tableau trié

   Votre réponse :

28. La programmation dynamique est pertinente lorsque :
    A. Les sous-problèmes sont indépendants
    B. La mémoire est limitée
    C. Il n’y a pas de récursivité
    D. Les sous-problèmes se recouvrent

   Votre réponse :

29. La mémoïsation est une approche :
    A. Top-down
    B. Bottom-up
    C. Itérative uniquement
    D. Gloutonne

   Votre réponse :

30. La tabulation est une approche :
    A. Récursive
    B. Top-down
    C. Bottom-up
    D. Exhaustive

   Votre réponse :

31. La complexité de Fibonacci naïf est :
    A. O(n)
    B. O(log n)
    C. O(2^n)
    D. O(n log n)

   Votre réponse :

32. Fibonacci avec programmation dynamique est en :
    A. O(2^n)
    B. O(n²)
    C. O(log n)
    D. O(n)

   Votre réponse :

33. Le problème du sac à dos 0/1 est :
    A. Polynomial trivial
    B. NP-complet (intuition)
    C. Toujours glouton
    D. Linéaire

   Votre réponse :

34. La programmation dynamique du sac à dos utilise :
    A. Une pile
    B. Une file
    C. Une table de hachage
    D. Une table 2D

   Votre réponse :

35. Une sous-structure optimale signifie que :
    A. Le problème n’a qu’une solution
    B. Une solution optimale contient des solutions optimales de sous-problèmes
    C. Les sous-problèmes sont indépendants
    D. Le problème est glouton

   Votre réponse :

36. La récursion simple est inefficace car :
    A. Elle recalcule des sous-problèmes
    B. Elle consomme trop de mémoire uniquement
    C. Elle est interdite en C
    D. Elle est trop complexe à écrire

   Votre réponse :

37. Un algorithme en O(n log n) est toujours plus rapide qu’un O(n²) :
    A. Vrai
    B. Faux

   Votre réponse :

38. Le facteur constant est :
    A. Toujours négligeable
    B. Ignoré en algorithmique
    C. Plus important que la complexité
    D. Parfois important en pratique

   Votre réponse :

39. Une solution gloutonne :
    A. Donne parfois l’optimal
    B. Donne toujours l’optimal
    C. Explore tout l’espace
    D. Utilise une table

   Votre réponse :

40. P vs NP signifie essentiellement :
    A. Mémoire vs temps
    B. Facile à résoudre vs facile à vérifier
    C. Itératif vs récursif
    D. Séquentiel vs parallèle

   Votre réponse :



# PARTIE B – Analyse de code et complexité

Soit le code C suivant :

```c
int mystery(int n) {
    int count = 0;
    for (int i = 1; i <= n; i *= 2) {
        for (int j = 0; j < i; j++) {
            count++;
        }
    }
    return count;
}
```

### Questions

1. Combien de fois la boucle externe est-elle exécutée ? 

   Votre réponse :

2. Exprimer le nombre total d’incréments de `count` en fonction de `n`. 

   Votre réponse :

3. Donner la complexité temporelle asymptotique de cette fonction. 

   Votre réponse :

4. Justifier brièvement votre réponse. 

   Votre réponse :

---

# PARTIE C – Mini-projet guidé : Détection de composantes fortement connexes

## Contexte applicatif 

On considère une application de **gestion de dépendances logicielles** (modules, microservices, packages).

* Chaque **sommet** représente un module.
* Une **arête orientée A → B** signifie que *A dépend de B*.

Un ensemble de modules forme un **bloc problématique** s’ils dépendent tous les uns des autres directement ou indirectement.

### Question 1

1. Expliquer pourquoi un tel bloc correspond à une **composante fortement connexe** d’un graphe orienté.

   Votre réponse :

---

## Étape 1 – Rappel : DFS et pile

On parcourt le graphe en profondeur (DFS) à partir d’un sommet `v`.

On maintient :

* un tableau `visited[]`
* une **pile** contenant les sommets actifs du parcours

### Question 2

1. Expliquer le rôle d’un parcours DFS dans l’exploration d’un graphe orienté et pourquoi il répond à la problématique.

   Votre réponse :

2. Pourquoi l’utilisation d’une pile est-elle pertinente dans ce cas ?

   Votre réponse :

---

## Étape 2 – Numérotation des sommets (index)

Lors du DFS, chaque sommet visité reçoit un **numéro croissant** `index[v]` correspondant à son ordre de découverte.

### Question 3

1. Quel est l’intérêt d’attribuer un indice de découverte à chaque sommet ?

   Votre réponse :

2. Pourquoi ces indices doivent-ils être uniques et croissants ?

   Votre réponse :

---

## Étape 3 – Propagation d’information (lowlink)

Pour chaque sommet `v`, on calcule une valeur `lowlink[v]` représentant :

> le plus petit indice atteignable depuis `v` en suivant des arêtes du graphe **et éventuellement en revenant en arrière dans le DFS**.

### Question 4

1. Expliquer intuitivement ce que représente `lowlink[v]`.

   Votre réponse :

2. Dans quel cas `lowlink[v]` peut-il être strictement inférieur à `index[v]` ?

   Votre réponse :

---

## Étape 4 – Détection d’une composante

À la fin du traitement d’un sommet `v`, on teste :

```c
if (lowlink[v] == index[v])
```

### Question 5

1. Que signifie cette condition sur le plan du graphe ?

   Votre réponse :

2. Pourquoi peut-on alors extraire plusieurs sommets de la pile pour former une composante fortement connexe ?

   Votre réponse :


---

## Étape 5 – Raisonnement algorithmique global

### Question 6

1. Expliquer pourquoi chaque sommet est empilé puis dépilé **une seule fois**.

   Votre réponse :

2. En déduire la complexité temporelle globale de l’algorithme en fonction de `V` et `E`.

   Votre réponse :

---

## Application concrète finale

Dans l’application de dépendances logicielles :

### Question 7

1. Citer **deux problèmes concrets** causés par la présence de composantes fortement connexes entre modules.

   Votre réponse :

2. Proposer **une action automatique** que pourrait réaliser l’outil après détection de ces composantes.

   Votre réponse :




**Fin de l’examen**
