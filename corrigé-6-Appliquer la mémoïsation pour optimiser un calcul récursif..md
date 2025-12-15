## Corrigé complet — Implémentations C

Les corrigés suivants sont fournis sous forme de **fichiers `.c` indépendants**.
Chaque fichier peut être compilé séparément.

---

# Fichier 1 — `knapsack.c`

```c
#include <stdio.h>
#include <string.h>

#define MAX_N 100
#define MAX_W 1000

int w[MAX_N+1], v[MAX_N+1];
int dp[MAX_N+1][MAX_W+1];
int n, W;

int max(int a, int b) {
    return (a > b) ? a : b;
}

int knapsack(int i, int c) {
    if (i == 0 || c == 0)
        return 0;

    if (dp[i][c] != -1)
        return dp[i][c];

    if (w[i] > c)
        dp[i][c] = knapsack(i - 1, c);
    else
        dp[i][c] = max(knapsack(i - 1, c),
                        v[i] + knapsack(i - 1, c - w[i]));

    return dp[i][c];
}

int main() {
    n = 4; W = 7;
    w[1] = 1; v[1] = 1;
    w[2] = 3; v[2] = 4;
    w[3] = 4; v[3] = 5;
    w[4] = 5; v[4] = 7;

    memset(dp, -1, sizeof(dp));

    printf("Valeur maximale = %d
", knapsack(n, W));
    return 0;
}
```

---

# Fichier 2 — `lcs.c`

```c
#include <stdio.h>
#include <string.h>

#define MAX_N 100
#define MAX_M 100

int max(int a, int b) {
    return (a > b) ? a : b;
}

int main() {
    char X[] = "ABCBDAB";
    char Y[] = "BDCABA";

    int n = strlen(X);
    int m = strlen(Y);

    int dp[MAX_N+1][MAX_M+1];

    for (int i = 0; i <= n; i++) dp[i][0] = 0;
    for (int j = 0; j <= m; j++) dp[0][j] = 0;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (X[i-1] == Y[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }

    printf("Longueur LCS = %d
", dp[n][m]);
    return 0;
}
```

---

# Fichier 3 — `grid_min_path.c`

```c
#include <stdio.h>

#define N 3
#define M 3

int min(int a, int b) {
    return (a < b) ? a : b;
}

int main() {
    int cost[N][M] = {
        {1, 3, 1},
        {1, 5, 1},
        {4, 2, 1}
    };

    int dp[N][M];

    dp[0][0] = cost[0][0];

    for (int i = 1; i < N; i++)
        dp[i][0] = dp[i-1][0] + cost[i][0];

    for (int j = 1; j < M; j++)
        dp[0][j] = dp[0][j-1] + cost[0][j];

    for (int i = 1; i < N; i++) {
        for (int j = 1; j < M; j++) {
            dp[i][j] = cost[i][j] + min(dp[i-1][j], dp[i][j-1]);
        }
    }

    printf("Coût minimal = %d
", dp[N-1][M-1]);
    return 0;
}
```

---

# Fichier 4 — `coin_change.c`

```c
#include <stdio.h>

#define MAX_M 1000

int main() {
    int coins[] = {1, 2, 5};
    int k = 3;
    int M = 5;

    long long dp[MAX_M+1] = {0};
    dp[0] = 1;

    for (int i = 0; i < k; i++) {
        for (int m = coins[i]; m <= M; m++) {
            dp[m] += dp[m - coins[i]];
        }
    }

    printf("Nombre de façons = %lld
", dp[M]);
    return 0;
}
```

## Remarques pédagogiques

* Chaque fichier illustre **une forme canonique** de programmation dynamique
* Les tailles sont volontairement bornées pour la lisibilité
* Tous les programmes sont compilables avec :

```bash
gcc -std=c99 fichier.c -o fichier
```

