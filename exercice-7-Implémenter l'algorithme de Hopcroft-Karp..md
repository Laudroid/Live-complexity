Bonjour à toutes et à tous,

Ce TP a pour objectif de vous faire implémenter un algorithme fondamental en théorie des graphes et en optimisation combinatoire : l'algorithme de Hopcroft-Karp. Cet algorithme est une référence pour trouver un couplage de cardinalité maximale dans un graphe biparti, avec une efficacité remarquable.

---

### **TP: Implémentation de l'Algorithme de Hopcroft-Karp pour l'Affectation de Tâches**

**Objectif du TP :** Implémenter et tester l'algorithme de Hopcroft-Karp pour trouver un couplage de cardinalité maximale dans un graphe biparti.

**Énoncé du TP :** Coder et tester l'algorithme de matching bipartite Hopcroft-Karp.

---

#### **Contexte & Problématique : L'Affectation Optimale de Tâches**

Imaginez une petite entreprise qui doit affecter des tâches spécifiques à ses employés. Chaque employé possède des compétences particulières et ne peut réaliser qu'une seule tâche à la fois. De même, chaque tâche ne peut être attribuée qu'à un seul employé. L'objectif est de maximiser le nombre total de tâches affectées.

Ce problème peut être modélisé comme un problème de couplage dans un graphe biparti :

*   **Ensemble $U$ :** Les employés.
*   **Ensemble $V$ :** Les tâches.
*   **Arête $(u, v)$ :** Existe si l'employé $u$ est compétent pour réaliser la tâche $v$.

Le but est de trouver un **couplage (matching)** de cardinalité maximale, c'est-à-dire un sous-ensemble d'arêtes tel qu'aucune arête ne partage un sommet, et dont le nombre d'arêtes est le plus grand possible.

#### **Rappels Théoriques Essentiels**

L'algorithme de Hopcroft-Karp est une amélioration des algorithmes basés sur la recherche de chemins augmentants. Sa complexité est en $O(E\sqrt{V})$, où $V$ est le nombre de sommets et $E$ le nombre d'arêtes. Il procède par phases :

1.  **Phase BFS (Breadth-First Search) :**
    *   Construit un ensemble de couches de sommets (un graphe de niveaux) à partir des sommets non couplés de $U$.
    *   Identifie la longueur $k$ du plus court chemin augmentant.
    *   Détermine si un chemin augmentant existe.
2.  **Phase DFS (Depth-First Search) :**
    *   Utilise le graphe de niveaux construit par le BFS pour trouver un ensemble maximal de chemins augmentants *disjoints* de longueur $k$.
    *   Met à jour le couplage pour chaque chemin augmentant trouvé.
3.  Ces deux phases sont répétées jusqu'à ce qu'aucun chemin augmentant ne puisse être trouvé.

#### **Travail à Réaliser**

Votre tâche consiste à implémenter l'algorithme de Hopcroft-Karp.

**Partie 1 : Représentation du Graphe Biparti**

1.  Choisissez une structure de données pour représenter le graphe biparti. Une **liste d'adjacence** est généralement la plus appropriée pour les algorithmes de graphes.
    *   Vous aurez besoin de deux ensembles de sommets distincts (employés $U$ et tâches $V$).
    *   Une fonction pour ajouter des arêtes entre un employé et une tâche.

**Partie 2 : Implémentation de l'Algorithme de Hopcroft-Karp**

Implémentez les fonctions clés de l'algorithme :

1.  **Variables Globales ou de Classe :**
    *   `matchU[u]` : Stocke la tâche $v$ à laquelle l'employé $u$ est couplé (ou `None`/`-1` si non couplé).
    *   `matchV[v]` : Stocke l'employé $u$ auquel la tâche $v$ est couplée (ou `None`/`-1` si non couplée).
    *   `dist[u]` : Stocke la distance (niveau) de l'employé $u$ dans le graphe de niveaux BFS.
    *   `NIL` : Une valeur spéciale (e.g., 0 ou -1) pour représenter un sommet non couplé ou non atteignable.
    *   `INF` : Une valeur pour l'infini.

2.  **Fonction `bfs()` :**
    *   Initialise `dist` à `INF` pour tous les sommets de $U$.
    *   Utilise une file (queue) pour la traversée.
    *   Ajoute tous les sommets de $U$ qui ne sont pas couplés (`matchU[u] == NIL`) à la file et leur assigne `dist[u] = 0`.
    *   Parcourt le graphe : pour chaque employé $u$ et chaque tâche $v$ adjacente à $u$ :
        *   Si la tâche $v$ est couplée à un employé `matchV[v]` et que `dist[matchV[v]] == INF`, cela signifie que `matchV[v]` n'a pas encore été visité dans cette phase BFS.
        *   Mettez à jour `dist[matchV[v]] = dist[u] + 1` et ajoutez `matchV[v]` à la file.
    *   Retourne `True` si un sommet non couplé de $V$ a été atteint (indiquant l'existence d'un chemin augmentant), `False` sinon.

3.  **Fonction `dfs(u)` :**
    *   Prend un employé `u` comme argument.
    *   Si `u` n'est pas `NIL` :
        *   Pour chaque tâche `v` adjacente à `u` :
            *   Si `dist[matchV[v]] == dist[u] + 1` (ce qui signifie que `matchV[v]` est au niveau suivant dans le graphe de niveaux) :
                *   Appelez récursivement `dfs(matchV[v])`. Si cet appel retourne `True` (un chemin augmentant a été trouvé à partir de là) :
                    *   Mettez à jour le couplage : `matchV[v] = u` et `matchU[u] = v`.
                    *   Retournez `True`.
        *   Si aucun chemin augmentant n'est trouvé à partir de `u`, mettez à jour `dist[u] = INF` pour éviter de le revisiter inutilement.
        *   Retournez `False`.
    *   Si `u` est `NIL`, cela signifie que nous avons atteint un sommet non couplé de $V$, donc un chemin augmentant a été trouvé. Retournez `True`.

4.  **Fonction `hopcroft_karp()` :**
    *   Initialise le couplage (`matchU`, `matchV`) à `NIL` pour tous les sommets.
    *   Initialise le compteur de couplages `result = 0`.
    *   Boucle tant que `bfs()` retourne `True` :
        *   Pour chaque employé `u` non couplé (`matchU[u] == NIL`) :
            *   Appelez `dfs(u)`. Si `dfs(u)` retourne `True`, incrémentez `result`.
    *   Retourne `result` (la cardinalité du couplage maximal).

**Partie 3 : Tests et Validation**

1.  **Jeu de Données :**
    *   **Employés (U) :** E1, E2, E3, E4
    *   **Tâches (V) :** T1, T2, T3, T4, T5
    *   **Compétences (Arêtes) :**
        *   (E1, T1), (E1, T2)
        *   (E2, T2), (E2, T3)
        *   (E3, T3), (E3, T4)
        *   (E4, T4), (E4, T5)

    *   **Représentation visuelle (pour vous aider) :**
        ```
        E1 -- T1
        |    /
        T2 -- E2
        |    /
        T3 -- E3
        |    /
        T4 -- E4
             |
             T5
        ```

2.  Exécutez votre algorithme sur ce jeu de données.
3.  Affichez la cardinalité du couplage maximal trouvé.
4.  Affichez le couplage lui-même (par exemple, "E1 est couplé à T1", "E2 est couplé à T2", etc.).
5.  Vérifiez manuellement la validité du couplage (chaque employé et chaque tâche n'apparaissent qu'une seule fois dans le couplage).

**Partie 4 : Analyse et Réflexion (Optionnel mais Recommandé)**

1.  **Complexité :** Confirmez la complexité théorique de $O(E\sqrt{V})$ et discutez brièvement des raisons de cette complexité par rapport à un algorithme de Ford-Fulkerson générique sur un réseau de flot.
2.  **Cas Limites :** Comment votre implémentation gère-t-elle un graphe vide, un graphe où aucun couplage n'est possible, ou un graphe où tous les sommets de $U$ peuvent être couplés ?
3.  **Applications :** Citez d'autres domaines d'application où un couplage biparti maximal pourrait être utile.

---

#### **Consignes Générales**

*   **Langage :** Vous êtes libre de choisir votre langage de programmation (Python, Java, C++, etc.). Python est souvent privilégié pour sa rapidité de prototypage et sa clarté.
*   **Clarté du Code :** Votre code doit être clair, bien structuré et commenté.
*   **Utilisation de l'IA :** N'hésitez pas à utiliser des outils d'IA générative pour vous aider à comprendre les concepts, débugger votre code, ou même générer des fragments de code. L'objectif est d'apprendre et de maîtriser l'algorithme, pas de lutter seul contre la machine. Soyez critiques envers les réponses obtenues et assurez-vous de bien comprendre ce que vous implémentez. L'IA est un outil, pas un substitut à votre compréhension.
*   **Rendu :** Le rendu attendu est votre code source, accompagné d'un court rapport (fichier texte ou PDF) présentant vos résultats pour le jeu de données fourni, ainsi que vos observations et réflexions de la Partie 4.

---

Bon courage pour cette implémentation ! C'est un excellent exercice pour solidifier votre compréhension des algorithmes de graphes et de la complexité. Amusez-vous bien !