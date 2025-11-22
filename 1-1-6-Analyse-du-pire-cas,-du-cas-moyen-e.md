# Fondements et rappels : Analyse du pire cas, du cas moyen et complexité amortie

## 2 - Analyse du pire cas, du cas moyen et complexité amortie

L’analyse de la complexité d’un algorithme ne se limite pas à une seule mesure absolue. Pour décrire finement ses performances, trois approches principales sont utilisées : l’analyse du pire cas, du cas moyen et la complexité amortie. Chaque méthode offre une perspective différente sur le comportement de l’algorithme selon les entrées et la fréquence des opérations.

---

## Analyse du pire cas (Worst-case)

- **Définition** : Mesure du temps d’exécution maximal pris par un algorithme pour n’importe quelle entrée de taille \(n\).  
- **Utilité** : Fournit une garantie absolue que l’algorithme ne dépassera jamais ce temps, utile pour des applications temps réel ou critiques.  
- **Exemple** :  
  - Recherche linéaire dans une liste non triée : pire cas = \(O(n)\), lorsque l’élément recherché est absent ou en fin de liste.

---

## Analyse du cas moyen (Average-case)

- **Définition** : Durée moyenne d’exécution d’un algorithme en supposant une distribution probabiliste donnée sur l’ensemble des entrées de taille \(n\).  
- **Précision** : Plus proche de la performance réelle mais dépend fortement de la modélisation statistique des entrées (souvent hypothèse d’entrées uniformément distribuées).  
- **Exemple** :  
  - Tri rapide (Quicksort) a un temps moyen en \(O(n \log n)\), même si son pire cas est \(O(n^2)\).  
  - Recherche dans une table de hachage : temps moyen constant \(O(1)\), pire cas linéaire \(O(n)\).

---

## Complexité amortie

- **Définition** : Temps moyen d’une opération pris sur une séquence d’opérations, lissant les coûts élevés ponctuels sur la totalité des opérations.  
- **Objectif** : Éviter de se focaliser sur un coût ponctuel élevé mais rare.  
- **Méthodes d’analyse** : Comptage, potentiel, banque.  
- **Exemple** :  
  - Table de hachage dynamique où la réallocation coûteuse est compensée par de nombreuses insertions rapides.  
  - Insertion dans une pile avec opération `push` en \(O(1)\) et `resize` occasionnel en \(O(n)\), amorti à \(O(1)\) par insertion.

---

## Illustrations par exemples

| Type d’analyse    | Exemple                     | Complexité typique          |
|-------------------|-----------------------------|----------------------------|
| Pire cas          | Tri fusion                  | \(O(n \log n)\)            |
| Cas moyen         | Quicksort (supposé aléatoire) | \(O(n \log n)\)            |
| Complexité amortie| Table de hachage (réallocation) | \(O(1)\) amorti par insertion |

---

## Diagramme Mermaid : comparaison des analyses

```mermaid
flowchart TD
    A[Opérations] --> B[Pire Cas (max temps)]
    A --> C[Cas Moyen (temps moyen)]
    A --> D[Complexité Amortie (moyenne sur séquence)]
    
    B --> E[P. Linéaire dans liste non triée]
    C --> F[Quicksort \(O(n \log n)\) moyen]
    D --> G[Table de hachage, insertion \(O(1)\) amorti]
```

---

## Synthèse

- Le pire cas fournit une borne supérieure sûre mais parfois pessimiste.  
- Le cas moyen représente la performance espérée sous hypothèses statistiques.  
- La complexité amortie évalue l’effort moyen par opération dans une séquence, souvent plus proche de la réalité dans des structures avec opérations coûteuses occasionnelles.

---

## Sources et références

- [Introduction to Algorithms, Cormen et al., Sections 2.1-2.3 (MIT Press)](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)  
- [Worst, Average and Amortized Analysis - GeeksforGeeks](https://www.geeksforgeeks.org/worst-average-and-amortized-analysis/)  
- [Analysis of algorithms - Wikipedia](https://en.wikipedia.org/wiki/Analysis_of_algorithms#Average_case)  
- [Amortized analysis - Wikipedia](https://en.wikipedia.org/wiki/Amortized_analysis)  

---

Cet article synthétise les différentes approches d’analyse de complexité, permettant d’appréhender plus précisément l’efficacité réelle d’un algorithme selon le contexte d’utilisation, la distribution des données, et la nature des opérations effectuées.