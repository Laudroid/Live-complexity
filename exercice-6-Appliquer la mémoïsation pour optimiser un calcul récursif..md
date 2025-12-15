Bonjour à toutes et à tous,

# TP guidé — Programmation dynamique

## Memoization et Tabulation (Algorithmique avancée)

## Objectifs pédagogiques

À l’issue de ce TP, vous serez capables de :

* Identifier les **sous-problèmes** d’un problème de programmation dynamique
* Vérifier les propriétés de **sous-structure optimale** et de **sous-problèmes chevauchants**
* Mettre en œuvre :
  * une approche **top-down avec memoization**
  * une approche **bottom-up avec tabulation**
* Analyser la **complexité temporelle et spatiale**

---

## Consignes générales

* Le TP est composé de **4 parties indépendantes**
* Les structures de données utilisées sont (C) :

  * tableaux statiques ou dynamiques (`malloc` / `free`)
  * tableaux 2D pour les tables de programmation dynamique
* Pour chaque exercice :

  * suivre les étapes guidées
  * écrire des **fonctions en C clairement typées**
  * justifier les choix (états, transitions, ordre de calcul)

---

# Partie 1 — Sac à dos 0/1 (Memoization)

## Problème

On dispose de `n` objets caractérisés par :

* un poids `w[i]`
* une valeur `v[i]`

La capacité maximale du sac est `W`. Chaque objet peut être pris **au plus une fois**.

---

## Étape 1 — Définition du sous-problème

On définit :

> **DP(i, c)** = valeur maximale atteignable avec les `i` premiers objets et une capacité restante `c`

En C, on utilisera une **table de mémoïsation** :

```c
int dp[MAX_N+1][MAX_W+1];
```

Initialiser la table avec `-1` pour indiquer un état non calculé.

---

## Étape 2 — Fonction récursive

Prototype attendu :

```c
int knapsack(int i, int c);
```

Règles :

* si `dp[i][c] != -1`, retourner la valeur mémorisée
* sinon calculer la valeur selon la relation de récurrence

---

## Étape 3 — Cas de base

```c
if (i == 0 || c == 0) return 0;
```

---

## Étape 4 — Appel principal

```c
int result = knapsack(n, W);
```

---

## Étape 5 — Analyse

* Temps : `O(n × W)`
* Espace : `O(n × W)`

---


# Partie 2 — Plus longue sous-séquence commune (Tabulation)

## Problème

Étant données deux chaînes :

* `char X[]` de longueur `n`
* `char Y[]` de longueur `m`

---

## Étape 1 — Table DP

On définit :

```c
int dp[MAX_N+1][MAX_M+1];
```

> `dp[i][j]` = longueur de la LCS entre `X[0..i-1]` et `Y[0..j-1]`

---

## Étape 2 — Initialisation

```c
for (int i = 0; i <= n; i++) dp[i][0] = 0;
for (int j = 0; j <= m; j++) dp[0][j] = 0;
```

---

## Étape 3 — Remplissage

```c
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        if (X[i-1] == Y[j-1])
            dp[i][j] = dp[i-1][j-1] + 1;
        else
            dp[i][j] = (dp[i-1][j] > dp[i][j-1]) ? dp[i-1][j] : dp[i][j-1];
    }
}
```

---

## Étape 4 — Résultat

```c
int lcs_length = dp[n][m];
```

---

## Étape 5 — Analyse

* Temps : `O(n × m)`
* Espace : `O(n × m)`

---


# Partie 3 — Traversée de grille : chemin minimal (Tabulation)

## Problème

Grille `n × m` avec un coût `cost[i][j]`.

---

## Étape 1 — Table DP

```c
int dp[MAX_N][MAX_M];
```

> `dp[i][j]` = coût minimal pour atteindre la cellule `(i, j)`

---

## Étape 2 — Initialisation

```c
dp[0][0] = cost[0][0];
for (int i = 1; i < n; i++) dp[i][0] = dp[i-1][0] + cost[i][0];
for (int j = 1; j < m; j++) dp[0][j] = dp[0][j-1] + cost[0][j];
```

---

## Étape 3 — Remplissage

```c
for (int i = 1; i < n; i++) {
    for (int j = 1; j < m; j++) {
        dp[i][j] = cost[i][j] +
            ((dp[i-1][j] < dp[i][j-1]) ? dp[i-1][j] : dp[i][j-1]);
    }
}
```

---

## Étape 4 — Résultat

```c
int min_cost = dp[n-1][m-1];
```

---

## Étape 5 — Analyse

* Temps : `O(n × m)`
* Espace : `O(n × m)`

---

# Partie 4 — Rendre un montant avec des pièces

## Problème

Pièces disponibles : `int coins[k]`
Montant cible : `M`

---

## Étape 1 — Table DP

```c
long long dp[MAX_M+1];
```

> `dp[m]` = nombre de façons de former le montant `m`

---

## Étape 2 — Initialisation

```c
dp[0] = 1;
for (int m = 1; m <= M; m++) dp[m] = 0;
```

---

## Étape 3 — Remplissage

```c
for (int i = 0; i < k; i++) {
    for (int m = coins[i]; m <= M; m++) {
        dp[m] += dp[m - coins[i]];
    }
}
```

---

## Étape 4 — Résultat

```c
long long ways = dp[M];
```

---

## Étape 5 — Analyse

* Temps : `O(k × M)`
* Espace : `O(M)`

---

* Temps : `O(k × M)`
* Espace : `O(k × M)` (ou `O(M)` optimisé)


**Conseils Utiles :**

*   **Débogage :** Pour les grandes valeurs de N, une erreur dans la gestion de la mémoïsation peut entraîner des boucles infinies ou des erreurs de segmentation. Soyez méthodique dans votre débogage.
*   **Overflow :** Les nombres de Fibonacci croissent très vite. Assurez-vous d'utiliser `long long` pour éviter les dépassements de capacité, surtout pour N > 45.
*   **Initialisation :** L'initialisation correcte du tableau de mémoïsation est cruciale. Chaque case doit indiquer si le résultat a été calculé ou non.

Amusez-vous bien !