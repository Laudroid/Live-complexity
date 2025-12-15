
## Chapitre 1 : Solution 1 - Implémentation Standard

```c
// main_solution1.c

#include <stdio.h>
#include <stdlib.h> // Pour malloc, free, abs

// --- Partie 1 : Divide & Conquer - Implémentation du Merge Sort ---

/**
 * @brief Fusionne deux sous-tableaux triés en un seul sous-tableau trié.
 *        Les sous-tableaux sont arr[l...m] et arr[m+1...r].
 * @param arr Le tableau principal.
 * @param l L'indice de début de la première sous-partie.
 * @param m L'indice de fin de la première sous-partie.
 * @param r L'indice de fin de la seconde sous-partie.
 */
void merge(int arr[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1; // Taille de la première sous-partie
    int n2 = r - m;     // Taille de la seconde sous-partie

    // Créer des tableaux temporaires
    int *L = (int*) malloc(n1 * sizeof(int));
    int *R = (int*) malloc(n2 * sizeof(int));

    if (L == NULL || R == NULL) {
        perror("Erreur d'allocation mémoire dans merge");
        if (L) free(L);
        if (R) free(R);
        exit(EXIT_FAILURE);
    }

    // Copier les données dans les tableaux temporaires L[] et R[]
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    // Fusionner les tableaux temporaires dans arr[l...r]
    i = 0; // Indice initial du premier sous-tableau
    j = 0; // Indice initial du second sous-tableau
    k = l; // Indice initial du sous-tableau fusionné

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copier les éléments restants de L[], s'il y en a
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copier les éléments restants de R[], s'il y en a
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }

    // Libérer la mémoire des tableaux temporaires
    free(L);
    free(R);
}

/**
 * @brief Implémente l'algorithme de tri Merge Sort.
 * @param arr Le tableau à trier.
 * @param l L'indice de début du sous-tableau à trier.
 * @param r L'indice de fin du sous-tableau à trier.
 */
void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        // Trouver le point milieu
        int m = l + (r - l) / 2;

        // Trier récursivement les deux moitiés
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);

        // Fusionner les moitiés triées
        merge(arr, l, m, r);
    }
}

/**
 * @brief Affiche les éléments d'un tableau.
 * @param arr Le tableau à afficher.
 * @param size La taille du tableau.
 */
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// --- Partie 2 : Backtracking - Le Problème des N-Dames (pour N=4) ---

/**
 * @brief Vérifie si placer une reine à (row, col) est sûr.
 * @param board Le tableau représentant l'échiquier.
 * @param row La ligne actuelle.
 * @param col La colonne actuelle.
 * @param N La taille de l'échiquier.
 * @return 1 si sûr, 0 sinon.
 */
int isSafe(int board[], int row, int col) {
    // Vérifier cette colonne sur les lignes précédentes
    for (int i = 0; i < row; i++) {
        if (board[i] == col) {
            return 0; // Reine déjà dans cette colonne
        }
    }

    // Vérifier les diagonales sur les lignes précédentes
    for (int i = 0; i < row; i++) {
        // vérifie si elles sont sur la même diagonale
        if (abs(board[i] - col) == abs(i - row)) {
            return 0; // Reine déjà sur une diagonale
        }
    }

    return 1; // C'est sûr de placer une reine ici
}

/**
 * @brief Affiche une solution trouvée du problème des N-Dames.
 * @param board Le tableau représentant l'échiquier avec les positions des reines.
 * @param N La taille de l'échiquier.
 */
void printSolution(int board[], int N) {
    static int solutionCount = 0;
    solutionCount++;
    printf("\nSolution %d:\n", solutionCount);
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (board[i] == j) {
                printf("Q "); // Reine
            } else {
                printf(". "); // Case vide
            }
        }
        printf("\n");
    }
}

/**
 * @brief Fonction récursive pour résoudre le problème des N-Dames en utilisant le backtracking.
 * @param board Le tableau représentant l'échiquier.
 * @param row La ligne actuelle où nous essayons de placer une reine.
 * @param N La taille de l'échiquier.
 */
void solveNQueens(int board[], int row, int N) {
    // Cas de base : toutes les reines ont été placées avec succès
    if (row == N) {
        printSolution(board, N);
        return;
    }

    // Essayer de placer une reine dans chaque colonne de la ligne actuelle
    for (int col = 0; col < N; col++) {
        // Vérifier si placer une reine à (row, col) est sûr
        if (isSafe(board, row, col, N)) {
            // Placer la reine
            board[row] = col;

            // Appeler récursivement pour placer la reine suivante dans la ligne suivante
            solveNQueens(board, row + 1, N);

            // Pas besoin de "défaire" board[row] = col ici, car la prochaine itération
            // de la boucle ou le retour de la fonction écrasera cette valeur ou ignorera cette branche.
            // C'est le principe du backtracking implicite pour ce problème.
        }
    }
}

// --- Fonction main pour tester les deux parties ---
int main() {
    // --- Test du Merge Sort ---
    printf("--- Partie 1 : Merge Sort ---\n");
    int data[] = {12, 11, 13, 5, 6, 7, 1, 9, 8, 4, 10, 2, 3};
    int n = sizeof(data) / sizeof(data[0]);

    printf("Tableau original : ");
    printArray(data, n);

    mergeSort(data, 0, n - 1);

    printf("Tableau trié : ");
    printArray(data, n);

    // --- Test du Problème des N-Dames (N=4) ---
    printf("\n--- Partie 2 : Problème des N-Dames (N=4) ---\n");
    int N_queens = 4;
    int board[N_queens]; // Tableau pour stocker la colonne de la reine pour chaque ligne

    solveNQueens(board, 0, N_queens); // Commencer à placer les reines à partir de la ligne 0

    return 0;
}
```



### Explications et Commentaires de la Solution 1

*   **Merge Sort** :
    *   **`merge` fonction** : C'est la partie "combiner". Elle prend deux sous-tableaux déjà triés et les fusionne en un seul. Elle utilise deux tableaux temporaires (`L` et `R`) pour stocker les éléments des sous-tableaux. C'est une approche standard et efficace. La gestion de l'allocation dynamique (`malloc`/`free`) pour ces tableaux temporaires est cruciale pour éviter les fuites de mémoire et permettre de trier des tableaux de tailles variées.
    *   **`mergeSort` fonction** : C'est la partie "diviser pour régner". Elle est récursive. Le cas de base est un sous-tableau de 0 ou 1 élément (déjà trié). Sinon, elle divise le tableau en deux, appelle récursivement `mergeSort` sur chaque moitié, puis appelle `merge` pour les fusionner.
    *   **`printArray`** : Une fonction utilitaire simple pour afficher le contenu d'un tableau.
*   **Problème des N-Dames** :
    *   **Modélisation** : L'échiquier est représenté par un tableau 1D `board[N]`. `board[i]` stocke la colonne où la reine de la ligne `i` est placée.
    *   **`isSafe` fonction** : Cette fonction est le cœur de la logique de validation. Elle vérifie trois conditions pour une nouvelle reine à `(row, col)` :
        1.  **Même colonne** : Parcourt les lignes précédentes (`i < row`) pour voir si `board[i]` est égal à `col`.
        2.  **Même diagonale** : Parcourt les lignes précédentes. La condition `abs(board[i] - col) == abs(i - row)` est une astuce mathématique pour vérifier si deux positions `(i, board[i])` et `(row, col)` sont sur la même diagonale. `abs()` de `stdlib.h` est utilisé.
    *   **`printSolution` fonction** : Affiche une solution trouvée de manière visuelle, avec 'Q' pour les reines et '.' pour les cases vides. Un compteur statique est utilisé pour numéroter les solutions.
    *   **`solveNQueens` fonction** : C'est la fonction de backtracking récursive.
        *   **Cas de base** : Si `row == N`, toutes les reines ont été placées avec succès (on a atteint la dernière ligne et on a pu y placer une reine). On affiche la solution.
        *   **Étape récursive** : Pour la ligne `row` actuelle, elle essaie de placer une reine dans chaque colonne (`col` de `0` à `N-1`). Si `isSafe` retourne vrai, elle place la reine (`board[row] = col`) et appelle récursivement `solveNQueens` pour la ligne suivante (`row + 1`). Le backtracking est implicite ici : si l'appel récursif ne mène pas à une solution, la boucle `for` continue avec la colonne suivante, ou la fonction retourne, "défaissant" ainsi le choix précédent.
*   **`main` fonction** : Contient les appels de test pour les deux parties, affichant les résultats.

---

## Chapitre 2 : Solution 2 - Merge Sort avec Allocation Unique, N-Dames avec Compteur

Cette solution apporte quelques raffinements : une gestion de la mémoire optimisée pour le Merge Sort (allocation unique du tableau temporaire) et une version du N-Dames qui compte les solutions en plus de les afficher.



```c
// main_solution2.c

#include <stdio.h>
#include <stdlib.h> // Pour malloc, free, abs

// --- Partie 1 : Divide & Conquer - Implémentation du Merge Sort (avec allocation unique) ---

// Tableau temporaire global ou passé en paramètre pour éviter des allocations/désallocations répétées
static int *temp_arr_v2 = NULL;

/**
 * @brief Fusionne deux sous-tableaux triés en un seul sous-tableau trié.
 *        Utilise un tableau temporaire pré-alloué.
 * @param arr Le tableau principal.
 * @param l L'indice de début de la première sous-partie.
 * @param m L'indice de fin de la première sous-partie.
 * @param r L'indice de fin de la seconde sous-partie.
 */
void merge_v2(int arr[], int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;

    // Copier les données dans le tableau temporaire (partie gauche)
    for (i = 0; i < n1; i++)
        temp_arr_v2[i] = arr[l + i];
    // Copier les données dans le tableau temporaire (partie droite, décalée)
    for (j = 0; j < n2; j++)
        temp_arr_v2[n1 + j] = arr[m + 1 + j];

    i = 0; // Indice pour la première moitié dans temp_arr_v2
    j = n1; // Indice pour la seconde moitié dans temp_arr_v2
    k = l;  // Indice pour le tableau original arr

    while (i < n1 && j < n1 + n2) {
        if (temp_arr_v2[i] <= temp_arr_v2[j]) {
            arr[k] = temp_arr_v2[i];
            i++;
        } else {
            arr[k] = temp_arr_v2[j];
            j++;
        }
        k++;
    }

    while (i < n1) {
        arr[k] = temp_arr_v2[i];
        i++;
        k++;
    }

    while (j < n1 + n2) {
        arr[k] = temp_arr_v2[j];
        j++;
        k++;
    }
}

/**
 * @brief Implémente l'algorithme de tri Merge Sort.
 *        Nécessite que temp_arr_v2 soit alloué avant le premier appel.
 * @param arr Le tableau à trier.
 * @param l L'indice de début du sous-tableau à trier.
 * @param r L'indice de fin du sous-tableau à trier.
 */
void mergeSort_v2(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort_v2(arr, l, m);
        mergeSort_v2(arr, m + 1, r);
        merge_v2(arr, l, m, r);
    }
}

/**
 * @brief Affiche les éléments d'un tableau.
 * @param arr Le tableau à afficher.
 * @param size La taille du tableau.
 */
void printArray_v2(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// --- Partie 2 : Backtracking - Le Problème des N-Dames (pour N=4) avec compteur ---

/**
 * @brief Vérifie si placer une reine à (row, col) est sûr.
 * @param board Le tableau représentant l'échiquier.
 * @param row La ligne actuelle.
 * @param col La colonne actuelle.
 * @param N La taille de l'échiquier.
 * @return 1 si sûr, 0 sinon.
 */
int isSafe_v2(int board[], int row, int col, int N) {
    for (int i = 0; i < row; i++) {
        if (board[i] == col || abs(board[i] - col) == abs(i - row)) {
            return 0;
        }
    }
    return 1;
}

/**
 * @brief Affiche une solution trouvée du problème des N-Dames.
 * @param board Le tableau représentant l'échiquier avec les positions des reines.
 * @param N La taille de l'échiquier.
 */
void printSolution_v2(int board[], int N) {
    printf("\n");
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (board[i] == j) {
                printf("Q ");
            } else {
                printf(". ");
            }
        }
        printf("\n");
    }
}

/**
 * @brief Fonction récursive pour résoudre le problème des N-Dames en utilisant le backtracking.
 *        Compte et affiche les solutions.
 * @param board Le tableau représentant l'échiquier.
 * @param row La ligne actuelle où nous essayons de placer une reine.
 * @param N La taille de l'échiquier.
 * @param solutionCount Pointeur vers un compteur de solutions.
 */
void solveNQueens_v2(int board[], int row, int N, int *solutionCount) {
    if (row == N) {
        (*solutionCount)++;
        printf("Solution %d:", *solutionCount);
        printSolution_v2(board, N);
        return;
    }

    for (int col = 0; col < N; col++) {
        if (isSafe_v2(board, row, col, N)) {
            board[row] = col;
            solveNQueens_v2(board, row + 1, N, solutionCount);
            // Backtracking implicite : la reine est "retirée" en passant à la colonne suivante
            // ou en retournant de la fonction.
        }
    }
}

// --- Fonction main pour tester les deux parties ---
int main() {
    // --- Test du Merge Sort ---
    printf("--- Partie 1 (V2) : Merge Sort ---\n");
    int data_v2[] = {12, 11, 13, 5, 6, 7, 1, 9, 8, 4, 10, 2, 3};
    int n_v2 = sizeof(data_v2) / sizeof(data_v2[0]);

    // Allouer le tableau temporaire une seule fois
    temp_arr_v2 = (int*) malloc(n_v2 * sizeof(int));
    if (temp_arr_v2 == NULL) {
        perror("Erreur d'allocation mémoire pour temp_arr_v2 dans main");
        return EXIT_FAILURE;
    }

    printf("Tableau original : ");
    printArray_v2(data_v2, n_v2);

    mergeSort_v2(data_v2, 0, n_v2 - 1);

    printf("Tableau trié : ");
    printArray_v2(data_v2, n_v2);

    free(temp_arr_v2); // Libérer le tableau temporaire après utilisation

    // --- Test du Problème des N-Dames (N=4) ---
    printf("\n--- Partie 2 (V2) : Problème des N-Dames (N=4) ---\n");
    int N_queens_v2 = 4;
    int board_v2[N_queens_v2];
    int solutionCount_v2 = 0; // Compteur de solutions

    solveNQueens_v2(board_v2, 0, N_queens_v2, &solutionCount_v2);

    printf("\nNombre total de solutions trouvées pour N=%d : %d\n", N_queens_v2, solutionCount_v2);

    return 0;
}
```



### Explications et Commentaires de la Solution 2

*   **Merge Sort avec Allocation Unique** :
    *   **`temp_arr_v2` (global/statique)** : Le tableau temporaire nécessaire à la fonction `merge` est alloué une seule fois dans `main` (ou déclaré comme global/statique). Cela évite les allocations et désallocations répétées à chaque appel récursif de `merge`, ce qui peut améliorer les performances pour de très grands tableaux en réduisant l'overhead de `malloc`/`free`.
    *   **`merge_v2` fonction** : Elle utilise ce tableau `temp_arr_v2` pré-alloué. Les deux sous-tableaux sont copiés dans des sections distinctes de ce tableau temporaire avant la fusion.
    *   **`mergeSort_v2` fonction** : Son interface reste la même, mais elle dépend de l'initialisation de `temp_arr_v2` avant le premier appel.
    *   **`main` fonction** : Gère l'allocation et la libération de `temp_arr_v2`.
*   **Problème des N-Dames avec Compteur** :
    *   **`solveNQueens_v2` fonction** :
        *   Un pointeur vers un entier `solutionCount` est passé en paramètre. Cela permet de modifier un compteur unique dans `main` à travers les appels récursifs.
        *   Chaque fois qu'une solution est trouvée (`row == N`), le compteur est incrémenté (`(*solutionCount)++`), et la solution est affichée avec son numéro.
    *   **`main` fonction** : Déclare `solutionCount_v2` et le passe par adresse à `solveNQueens_v2`. À la fin, elle affiche le nombre total de solutions trouvées.
*   **Autres fonctions** : Les fonctions `isSafe_v2` et `printSolution_v2` sont fonctionnellement identiques à celles de la Solution 1, avec un suffixe `_v2` pour la distinction.
