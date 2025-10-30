
## TP : Analyse de Complexité Temporelle - Recherche et Tri Simples

### Objectif du TP

Ce TP vise à vous familiariser avec l'analyse de la complexité temporelle d'algorithmes, en distinguant les scénarios de pire cas et de cas moyen. Vous appliquerez cette analyse à des algorithmes fondamentaux : la recherche linéaire et le tri par insertion.

### Contexte

La performance d'un algorithme est une préoccupation centrale en informatique. Comprendre comment un algorithme se comporte en fonction de la taille de ses données d'entrée est essentiel pour concevoir des systèmes efficaces et évolutifs. Ce TP vous guidera à travers l'analyse formelle de cette performance.

### Prérequis

*   Bases de la programmation (boucles, tableaux/listes, fonctions).
*   Notions élémentaires de mathématiques discrètes (sommations, suites arithmétiques).
*   Familiarité avec la notation Grand O (O()).

### Matériel

*   Un environnement de développement (IDE) de votre choix (ex: VS Code, PyCharm, IntelliJ).
*   Un langage de programmation (Python, Java, C++, etc.).

---

### Énoncé du TP : Gestionnaire de Liste d'Entiers

Nous allons travailler sur un scénario simple : la gestion d'une liste de nombres entiers. Vous devrez implémenter et analyser des fonctions pour manipuler cette liste.

#### Partie 1 : Recherche Linéaire

La recherche linéaire est une méthode simple pour trouver un élément dans une liste en parcourant séquentiellement chaque élément jusqu'à ce que l'élément cible soit trouvé ou que la fin de la liste soit atteinte.

**1.1 Implémentation**
Écrivez une fonction `recherche_lineaire(liste, element_a_chercher)` qui prend en entrée une liste d'entiers et un entier à rechercher. La fonction doit renvoyer l'indice de l'élément s'il est trouvé, ou -1 sinon.

**1.2 Analyse du Pire Cas**
*   Décrivez précisément le scénario qui conduit au pire cas pour la recherche linéaire.
*   Calculez la complexité temporelle en pire cas en utilisant la notation Grand O (O()). Justifiez votre réponse en comptant les opérations élémentaires (comparaisons, accès à la liste).

**1.3 Analyse du Cas Moyen**
*   Décrivez un scénario typique pour le cas moyen. Quelles hypothèses faites-vous sur la distribution des éléments dans la liste et sur la probabilité de trouver l'élément ?
*   Calculez la complexité temporelle en cas moyen en notation Grand O. Justifiez votre réponse en détaillant votre raisonnement (par exemple, en considérant la position moyenne de l'élément s'il est trouvé).
    *   *Conseil :* N'hésitez pas à solliciter l'IA pour des rappels sur les formules de sommes arithmétiques ou pour structurer votre raisonnement sur les probabilités.

#### Partie 2 : Tri par Insertion

Le tri par insertion est un algorithme de tri simple qui construit la liste finale triée un élément à la fois. Il est efficace pour de petits ensembles de données ou des données presque triées.

**2.1 Implémentation**
Écrivez une fonction `tri_insertion(liste)` qui prend en entrée une liste d'entiers et la trie en place (modifie la liste originale).

**2.2 Analyse du Pire Cas**
*   Décrivez précisément le scénario qui conduit au pire cas pour le tri par insertion.
*   Calculez la complexité temporelle en pire cas en notation Grand O. Justifiez votre réponse en comptant les opérations élémentaires (comparaisons, décalages d'éléments).

**2.3 Analyse du Cas Moyen**
*   Décrivez un scénario typique pour le cas moyen. Quelles hypothèses faites-vous sur la distribution des éléments dans la liste non triée ?
*   Calculez la complexité temporelle en cas moyen en notation Grand O. Justifiez votre réponse.
    *   *Conseil :* L'analyse du cas moyen pour le tri par insertion peut être plus subtile. L'IA peut vous aider à explorer différentes approches ou à vérifier vos hypothèses sur le nombre moyen de décalages.

---

### Rendu

Vous devrez soumettre un rapport au format PDF incluant les éléments suivants :

1.  **Le code source** de vos implémentations de `recherche_lineaire` et `tri_insertion`, commenté de manière claire.
2.  **Les analyses détaillées** (pire cas et cas moyen) pour chaque algorithme, avec toutes les justifications nécessaires (comptage des opérations, raisonnement sur les hypothèses).
3.  **Les réponses aux questions de la Partie 3**.

Assurez-vous que votre rapport est clair, structuré et professionnel.

