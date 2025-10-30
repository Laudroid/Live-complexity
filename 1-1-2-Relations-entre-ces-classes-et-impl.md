# Fondements et rappels : Relations entre classes de complexité fondamentales et implications théoriques

## 2- Relations entre les classes P, NP, PSPACE, EXPTIME et implications

La théorie de la complexité étudie la classification des problèmes selon les ressources (temps, espace) nécessaires à leur résolution. Il est fondamental de comprendre non seulement chaque classe de complexité, mais aussi leurs relations et ce que ces relations impliquent en termes de potentiel et limitations des algorithmes.

---

## Relations d'inclusion principales

Les classes de complexité les plus étudiées sont liées par des inclusions toujours vraies, même si certaines égalités restent des questions ouvertes.

Les inclusions suivantes sont généralement admises et démontrées :

\[
P \subseteq NP \subseteq PSPACE \subseteq EXPTIME
\]

- **P \(\subseteq\) NP** : Tout problème résolvable en temps polynomial peut également être vérifié en temps polynomial, donc P est inclus dans NP.

- **NP \(\subseteq\) PSPACE** : Tous les problèmes vérifiables en temps polynomial utilisent au maximum un espace polynomial. Il est possible de simuler une machine non déterministe avec espace polynomial par une machine déterministe utilisant également espace polynomial.

- **PSPACE \(\subseteq\) EXPTIME** : Un problème résoluble avec un espace polynomial peut être résolu, en temps exponentiel dans le pire cas, par une machine déterministe.

Ces inclusions sont strictes dans la majorité des cas, mais leur démonstration rigoureuse est souvent encore ouverte, notamment la célèbre question P = NP, qui reste non résolue.

---

## Implications théoriques majeures

1. **Si P = NP, alors P = PSPACE = EXPTIME**

   Une égalité P=NP entrainerait une simplification majeure de toute la hiérarchie connue puisque cela signifierait que les problèmes considérés intrinsèquement difficiles seraient en fait solubles efficacement.

2. **Exemple de problème PSPACE-complet**

   La résolution de jeux à deux joueurs avec un nombre polynomial de coups (exemple : hex) est PSPACE-complet, c’est-à-dire au cœur de la classe PSPACE. Ces jeux sont en général encore plus difficiles que ceux de classe NP.

3. **Les classes co-NP et NP**

   co-NP est la classe des problèmes dont les NON solutions peuvent être vérifiées en temps polynomial. Une question célèbre est de savoir si NP = co-NP, ce qui impacterait encore la compréhension des problèmes dits "difficiles".

---

## Exemples pour illustrer

| Classe     | Exemple de problème                                      | Ressource critique           |
|------------|----------------------------------------------------------|-----------------------------|
| P          | Recherche dans liste triée (recherche binaire)           | Temps polynomial            |
| NP         | Problème de satisfiabilité (SAT)                         | Vérification rapide         |
| PSPACE     | Jeu de Hex, TQBF (Théorie des quantificateurs booléens) | Espace polynomial           |
| EXPTIME    | Jeu d’échecs généralisé (plateau arbitraire)            | Temps exponentiel           |

---

## Diagramme Mermaid des inclusions

```mermaid
graph LR
    P --> NP
    NP --> PSPACE
    PSPACE --> EXPTIME
    subgraph co-NP
        direction LR
        NP --- co-NP
    end
```

Ce diagramme visualise la relation d'inclusion entre les classes avec la mention de co-NP chevauchant NP. Notez que l'égalité NP=co-NP est une question ouverte.

---

## Notes complémentaires

- Le fait que PSPACE soit égal à NPSPACE (machines non déterministes utilisant un espace polynomial) est démontré, ce qui est une rare égalité entre classes déterministes et non déterministes en espace.

- EXPTIME contient strictement les classes de temps polynomial, mais on ignore si des inclusions strictes existent entre P, NP, et PSPACE (sauf PSPACE \(\subseteq\) EXPTIME connu).

---

## Sources utilisées

- [Classe de complexité - Wikipédia](https://fr.wikipedia.org/wiki/Classe_de_complexit%C3%A9)  
- [Co-NP, PSPACE, and EXPTIME Explained Simply - Medium](https://soumenatta.medium.com/co-np-pspace-and-exptime-explained-simply-beyond-np-in-complexity-theory-fc3cd54f38fd)  
- [P (complexité) - Wikipédia](https://fr.wikipedia.org/wiki/P_(complexit%C3%A9))  
- [Leçon 915 : Classes de complexité. Exemples - ENS Rennes](https://perso.eleves.ens-rennes.fr/people/julie.parreaux/fichiers_agreg/info_lecons/915_ClassesComplexites.pdf)  

---

Cet article permet de comprendre les relations fondamentales entre classes classiques de la complexité computationnelle, soulignant leurs inclusions, les questions ouvertes majeures et les implications profondes sur ce qui est calculable efficacement ou non. Connaître ces frontières est déterminant pour analyser la difficulté intrinsèque des problèmes d’algorithmique.