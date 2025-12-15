Voici deux solutions possibles pour le TP sur l'algorithme de Hopcroft-Karp, présentées sous forme de chapitres distincts.

---

# Chapitre 1 : Solution Détaillée (Implémentation Python)

Ce chapitre propose une implémentation complète de l'algorithme de Hopcroft-Karp en Python, avec des explications détaillées pour chaque partie.

## Partie 1 : Représentation du Graphe Biparti

Nous utiliserons une liste d'adjacence pour représenter le graphe. Les employés et les tâches seront représentés par des entiers pour simplifier l'indexation.


```python
from collections import deque

class BipartiteGraph:
    def __init__(self, num_U, num_V):
        """
        Initialise un graphe biparti.
        num_U: Nombre de sommets dans l'ensemble U (employés).
        num_V: Nombre de sommets dans l'ensemble V (tâches).
        """
        self.num_U = num_U
        self.num_V = num_V
        self.adj = [[] for _ in range(num_U + 1)] # Liste d'adjacence pour les employés (U)
                                                  # Les indices 0 sont ignorés pour U et V pour simplifier

    def add_edge(self, u, v):
        """
        Ajoute une arête entre un employé u et une tâche v.
        u: indice de l'employé (1 à num_U)
        v: indice de la tâche (1 à num_V)
        """
        self.adj[u].append(v)

```


## Partie 2 : Implémentation de l'Algorithme de Hopcroft-Karp

L'implémentation suit les phases BFS et DFS décrites.


```python
class HopcroftKarp:
    def __init__(self, graph):
        self.graph = graph
        self.NIL = 0 # Représente un sommet non couplé ou non atteignable
        self.INF = float('inf') # Représente l'infini

        # matchU[u] = v si u est couplé à v, NIL sinon.
        # Les indices de U vont de 1 à graph.num_U
        self.matchU = [self.NIL] * (graph.num_U + 1) 
        
        # matchV[v] = u si v est couplé à u, NIL sinon.
        # Les indices de V vont de 1 à graph.num_V
        self.matchV = [self.NIL] * (graph.num_V + 1) 
        
        # dist[u] = distance (niveau) de u dans le graphe de niveaux BFS
        self.dist = [self.INF] * (graph.num_U + 1)

    def bfs(self):
        """
        Phase BFS: Construit le graphe de niveaux et trouve le plus court chemin augmentant.
        Retourne True si un chemin augmentant est trouvé, False sinon.
        """
        queue = deque()

        # Initialise les distances et ajoute les sommets non couplés de U à la file
        for u in range(1, self.graph.num_U + 1):
            if self.matchU[u] == self.NIL:
                self.dist[u] = 0
                queue.append(u)
            else:
                self.dist[u] = self.INF # Les sommets couplés sont initialement à l'infini

        self.dist[self.NIL] = self.INF # Le NIL est à l'infini

        while queue:
            u = queue.popleft()

            if self.dist[u] < self.dist[self.NIL]: # Si u n'est pas au niveau INF
                for v in self.graph.adj[u]:
                    # Si la tâche v est couplée à matchV[v] et que matchV[v] n'a pas été visité
                    if self.dist[self.matchV[v]] == self.INF:
                        self.dist[self.matchV[v]] = self.dist[u] + 1
                        queue.append(self.matchV[v])
        
        # Retourne True si un chemin augmentant a été trouvé (i.e., NIL est atteignable)
        return self.dist[self.NIL] != self.INF

    def dfs(self, u):
        """
        Phase DFS: Trouve un chemin augmentant à partir de u en utilisant le graphe de niveaux.
        Retourne True si un chemin augmentant est trouvé et le couplage est mis à jour, False sinon.
        """
        if u != self.NIL:
            for v in self.graph.adj[u]:
                # Si v est au niveau suivant dans le graphe de niveaux
                if self.dist[self.matchV[v]] == self.dist[u] + 1:
                    # Tente de trouver un chemin augmentant à partir de matchV[v]
                    if self.dfs(self.matchV[v]):
                        # Si un chemin est trouvé, met à jour le couplage
                        self.matchV[v] = u
                        self.matchU[u] = v
                        return True
            
            # Si aucun chemin augmentant n'est trouvé à partir de u, marquez-le comme non atteignable
            self.dist[u] = self.INF
            return False
        return True # Si u est NIL, un chemin augmentant a été trouvé

    def hopcroft_karp_algorithm(self):
        """
        Exécute l'algorithme de Hopcroft-Karp.
        Retourne la cardinalité du couplage maximal.
        """
        matching_size = 0
        
        # Répète les phases BFS et DFS tant que des chemins augmentants sont trouvés
        while self.bfs():
            for u in range(1, self.graph.num_U + 1):
                # Pour chaque sommet non couplé de U, tente de trouver un chemin augmentant
                if self.matchU[u] == self.NIL and self.dfs(u):
                    matching_size += 1
        
        return matching_size

```


## Partie 3 : Tests et Validation

Appliquons l'algorithme au jeu de données fourni.


```python
# --- Jeu de Données ---
# Employés (U): E1, E2, E3, E4 (num_U = 4)
# Tâches (V): T1, T2, T3, T4, T5 (num_V = 5)

num_employees = 4
num_tasks = 5
b_graph = BipartiteGraph(num_employees, num_tasks)

# Compétences (Arêtes)
b_graph.add_edge(1, 1) # E1 -> T1
b_graph.add_edge(1, 2) # E1 -> T2
b_graph.add_edge(2, 2) # E2 -> T2
b_graph.add_edge(2, 3) # E2 -> T3
b_graph.add_edge(3, 3) # E3 -> T3
b_graph.add_edge(3, 4) # E3 -> T4
b_graph.add_edge(4, 4) # E4 -> T4
b_graph.add_edge(4, 5) # E4 -> T5

# Exécution de l'algorithme
hk_solver = HopcroftKarp(b_graph)
max_matching_size = hk_solver.hopcroft_karp_algorithm()

print(f"Cardinalité du couplage maximal: {max_matching_size}")

print("\nCouplage trouvé:")
for u in range(1, num_employees + 1):
    if hk_solver.matchU[u] != hk_solver.NIL:
        print(f"  Employé E{u} est couplé à Tâche T{hk_solver.matchU[u]}")
    else:
        print(f"  Employé E{u} n'est pas couplé")

# Vérification manuelle:
# Un couplage possible de cardinalité 4:
# E1-T1, E2-T2, E3-T3, E4-T4 (T5 non couplée)
# Ou E1-T1, E2-T3, E3-T4, E4-T5 (T2 non couplée)
# Ou E1-T2, E2-T3, E3-T4, E4-T5 (T1 non couplée)
# Le résultat de l'algorithme devrait être 4.
```


**Résultat attendu :**
*   Cardinalité du couplage maximal : 4
*   Un exemple de couplage affiché pourrait être :
    *   Employé E1 est couplé à Tâche T1
    *   Employé E2 est couplé à Tâche T2
    *   Employé E3 est couplé à Tâche T3
    *   Employé E4 est couplé à Tâche T4
    *(L'ordre des couplages peut varier, mais la cardinalité sera 4.)*

## Partie 4 : Analyse et Réflexion

### 4.1 Complexité

L'algorithme de Hopcroft-Karp a une complexité temporelle de **$O(E\sqrt{V})$**, où $V$ est le nombre total de sommets ($num\_U + num\_V$) et $E$ est le nombre d'arêtes.
Cette complexité est meilleure que celle d'un algorithme de Ford-Fulkerson générique sur un réseau de flot (qui serait $O(FE)$ où $F$ est la valeur du flot maximal, ou $O(VE^2)$ avec Edmonds-Karp). La raison de cette amélioration est que Hopcroft-Karp ne trouve pas un seul chemin augmentant par itération, mais un ensemble maximal de chemins augmentants *disjoints* de longueur minimale à chaque phase BFS/DFS. Cela réduit le nombre de phases nécessaires à $O(\sqrt{V})$.

### 4.2 Cas Limites

*   **Graphe vide :** L'algorithme gère correctement un graphe vide (aucun employé, aucune tâche, ou aucune compétence). `num_U` et `num_V` seraient 0, les boucles ne s'exécuteraient pas, et le résultat serait 0.
*   **Aucun couplage possible :** Si aucun employé n'a de compétence pour aucune tâche, `bfs()` ne trouvera jamais de chemin augmentant, et `hopcroft_karp_algorithm()` retournera 0.
*   **Tous les sommets de U peuvent être couplés :** Si chaque employé peut être couplé à une tâche unique, l'algorithme trouvera un couplage de taille `num_U`.

### 4.3 Applications

Le couplage biparti maximal a de nombreuses applications au-delà de l'affectation de tâches :
*   **Planification et ordonnancement :** Affecter des machines à des travaux, des enseignants à des cours.
*   **Réseaux :** Problèmes de routage, de connexion.
*   **Bio-informatique :** Alignement de séquences, analyse de réseaux de protéines.
*   **Vision par ordinateur :** Reconnaissance d'objets, appariement de caractéristiques.
*   **Recherche opérationnelle :** Optimisation de ressources.

---

# Chapitre 2 : Solution Alternative (Implémentation Python avec `networkx`)

Ce chapitre propose une solution plus concise en utilisant la bibliothèque `networkx` de Python, qui offre une implémentation optimisée de l'algorithme de couplage biparti.

## Partie 1 : Représentation du Graphe Biparti

Nous utiliserons la structure de graphe de `networkx` qui gère naturellement les graphes bipartis.


```python
import networkx as nx

class BipartiteGraphNX:
    def __init__(self, U_nodes, V_nodes):
        """
        Initialise un graphe biparti pour networkx.
        U_nodes: Liste des nœuds de l'ensemble U (employés).
        V_nodes: Liste des nœuds de l'ensemble V (tâches).
        """
        self.graph = nx.Graph()
        self.graph.add_nodes_from(U_nodes, bipartite=0) # 0 pour l'ensemble U
        self.graph.add_nodes_from(V_nodes, bipartite=1) # 1 pour l'ensemble V
        self.U_nodes = U_nodes
        self.V_nodes = V_nodes

    def add_edge(self, u, v):
        """
        Ajoute une arête entre un nœud de U et un nœud de V.
        """
        if (u in self.U_nodes and v in self.V_nodes) or \
           (u in self.V_nodes and v in self.U_nodes): # networkx gère les arêtes non orientées
            self.graph.add_edge(u, v)
        else:
            raise ValueError("L'arête doit connecter un nœud de U et un nœud de V.")

```


## Partie 2 : Implémentation de l'Algorithme de Hopcroft-Karp

`networkx` fournit une fonction directe pour le couplage biparti maximal.


```python
def hopcroft_karp_networkx(bipartite_graph_nx_instance):
    """
    Utilise la fonction intégrée de networkx pour trouver le couplage maximal.
    """
    # networkx.bipartite.maximum_matching implémente Hopcroft-Karp ou un algorithme équivalent.
    matching = nx.bipartite.maximum_matching(bipartite_graph_nx_instance.graph, 
                                             bipartite_graph_nx_instance.U_nodes)
    return matching

```


## Partie 3 : Tests et Validation

Appliquons la solution `networkx` au jeu de données fourni.


```python
# --- Jeu de Données ---
# Employés (U): E1, E2, E3, E4
# Tâches (V): T1, T2, T3, T4, T5

employees = ['E1', 'E2', 'E3', 'E4']
tasks = ['T1', 'T2', 'T3', 'T4', 'T5']

b_graph_nx = BipartiteGraphNX(employees, tasks)

# Compétences (Arêtes)
b_graph_nx.add_edge('E1', 'T1')
b_graph_nx.add_edge('E1', 'T2')
b_graph_nx.add_edge('E2', 'T2')
b_graph_nx.add_edge('E2', 'T3')
b_graph_nx.add_edge('E3', 'T3')
b_graph_nx.add_edge('E3', 'T4')
b_graph_nx.add_edge('E4', 'T4')
b_graph_nx.add_edge('E4', 'T5')

# Exécution de l'algorithme
matching_result = hopcroft_karp_networkx(b_graph_nx)

# Le résultat de networkx est un dictionnaire où chaque clé est un sommet couplé,
# et sa valeur est le sommet auquel il est couplé.
# Pour afficher le couplage de manière claire, on peut filtrer les paires (u,v)
# où u est dans l'ensemble U.

max_matching_size_nx = 0
print("Couplage trouvé:")
for u_node in employees:
    if u_node in matching_result:
        v_node = matching_result[u_node]
        # Vérifie que v_node est bien une tâche et qu'il n'a pas déjà été affiché
        # (networkx donne les deux sens du couplage, ex: {'E1': 'T1', 'T1': 'E1'})
        if v_node in tasks:
            print(f"  Employé {u_node} est couplé à Tâche {v_node}")
            max_matching_size_nx += 1

print(f"\nCardinalité du couplage maximal: {max_matching_size_nx}")

```


**Résultat attendu :**
*   Cardinalité du couplage maximal : 4
*   Un exemple de couplage affiché pourrait être :
    *   Employé E1 est couplé à Tâche T1
    *   Employé E2 est couplé à Tâche T2
    *   Employé E3 est couplé à Tâche T3
    *   Employé E4 est couplé à Tâche T4
    *(L'ordre et les paires exactes peuvent varier, mais la cardinalité sera 4.)*

## Partie 4 : Analyse et Réflexion

### 4.1 Complexité

La fonction `nx.bipartite.maximum_matching` utilise une implémentation optimisée de l'algorithme de Hopcroft-Karp ou un algorithme équivalent. Sa complexité théorique est bien de $O(E\sqrt{V})$, offrant une efficacité supérieure aux algorithmes de flot maximal basés sur des chemins augmentants simples pour les graphes bipartis.

### 4.2 Cas Limites

`networkx` gère robustement les cas limites :
*   **Graphe vide :** Retourne un dictionnaire vide, taille 0.
*   **Aucun couplage possible :** Retourne un dictionnaire vide.
*   **Tous les sommets de U peuvent être couplés :** Retourne un couplage de taille `len(U_nodes)`.

### 4.3 Applications

Les applications sont les mêmes que celles mentionnées dans la solution détaillée : affectation de ressources, planification, bio-informatique, vision par ordinateur, etc. L'utilisation de bibliothèques comme `networkx` permet aux développeurs de se concentrer sur la modélisation du problème et l'interprétation des résultats plutôt que sur l'implémentation complexe de l'algorithme sous-jacent.

---