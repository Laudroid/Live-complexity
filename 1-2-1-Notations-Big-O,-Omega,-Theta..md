# Fondements et rappels : Notations asymptotiques - Big O, Omega, Theta

## 1- Notations Big O, Omega, Theta

L'analyse de la complexité algorithmique s'appuie sur des notations asymptotiques qui permettent de décrire le comportement de fonctions liées au temps ou à l'espace nécessaires pour résoudre un problème en fonction de la taille de l'entrée. Ces notations simplifient la comparaison des algorithmes en négligeant les constantes et les termes de moindre ordre.

---

## Définitions formelles

Soient \(f(n)\) et \(g(n)\) deux fonctions définies pour \(n \geq n_0\), et positives.

### Big O : majoration asymptotique (limite supérieure)

- **Notation** : \(f(n) = O(g(n))\)
- **Définition** : \(f\) est en Big O de \(g\) s'il existe des constantes positives \(c\) et \(n_0\) telles que, pour tout \(n \geq n_0\),
  
  \[
  0 \leq f(n) \leq c \cdot g(n)
  \]
  
- **Interprétation** : \(f\) croît au plus comme \(g\), jusqu’à une constante multiplicative à partir d’un certain rang.

### Omega : minoration asymptotique (limite inférieure)

- **Notation** : \(f(n) = \Omega(g(n))\)
- **Définition** : \(f\) est en Omega de \(g\) s'il existe \(c > 0\), \(n_0\) tel que, pour tout \(n \geq n_0\),
  
  \[
  0 \leq c \cdot g(n) \leq f(n)
  \]
  
- **Interprétation** : \(f\) croît au moins comme \(g\) à partir d’un certain point.

### Theta : croissance asymptotiquement équivalente (majoration et minoration)

- **Notation** : \(f(n) = \Theta(g(n))\)
- **Définition** : \(f\) est en Theta de \(g\) s'il existe deux constantes \(c_1, c_2 > 0\), \(n_0\) telles que, pour tout \(n \geq n_0\),
  
  \[
  c_1 \cdot g(n) \leq f(n) \leq c_2 \cdot g(n)
  \]
  
- **Interprétation** : \(f\) et \(g\) ont le même ordre de grandeur asymptotique.

---

## Exemples concrets

1. **Fonction linéaire et quadratique**

- \(f(n) = 5n + 3\)  
On a \(f(n) = O(n)\) (le terme linéaire domine), aussi \(f(n) = \Omega(n)\), donc \(f(n) = \Theta(n)\).

- \(h(n) = 3n^2 + 2n + 1\)  
Ici, \(h(n) = O(n^2)\) et \(h(n) = \Omega(n^2)\) donc \(h(n) = \Theta(n^2)\).

2. **Comparaison différenciée**

- \(f(n) = n\)  
- \(g(n) = n \log n\)  
Nous avons \(f(n) = O(g(n))\) mais \(f(n) \neq \Omega(g(n))\) car \(n \log n\) croît plus vite que \(n\).

---

## Diagramme Mermaid : Illustration intuitive

```mermaid
flowchart LR
    A(f(n))
    B(Big O) --> A
    C(Omega) --> A
    B -- "limite supérieure" --> G[g(n)]
    C -- "limite inférieure" --> G[g(n)]
    D(Theta) --> B & C
```

Ce diagramme schématise que Big O est une borne supérieure, Omega une borne inférieure, et Theta les deux à la fois.

---

## Utilisation pratique

- **Big O** sert à affirmer une complexité maximale. Par exemple, dire « l’algorithme est \(O(n^2)\) » implique que le temps ne dépasse pas une fonction quadratique.

- **Omega** est utile pour démontrer des bornes minimales, par exemple, on sait que certains algorithmes de tri ont une borne inférieure \(\Omega(n \log n)\).

- **Theta** est la notation la plus précise, indiquant la complexité exacte à l’ordre près.

---

## Sources et références

- [Big O notation - Wikipedia](https://en.wikipedia.org/wiki/Big_O_notation)  
- [Asymptotic notations — GeeksforGeeks](https://www.geeksforgeeks.org/analysis-of-algorithms-set-1-asymptotic-analysis/)  
- [Notations Asymptotiques – Le Petit Aprentissage](https://www.lepetitapprenti.fr/math-info/complexite/not-notations-asymptotiques.html)  
- [Introduction to Algorithms, Cormen et al., Chapitre 3 (CLRS)](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)

---

Cet article fournit un éclairage clair et précis sur les notations Big O, Omega et Theta, outils indispensables pour caractériser la croissance des fonctions de complexité. Les définitions formelles et exemples concrets permettent de mieux saisir leur portée et leur usage dans l’évaluation des algorithmes.