# Complexité paramétrée et cas moyens : Présentation des learned indexes et leur usage en base de données

## 1 - Learned indexes : définition et application en bases de données

### Qu’est-ce qu’un learned index ?

Traditionnellement, les bases de données utilisent des structures d’index classiques comme les arbres B-tree pour accélérer la recherche de clés. Ces structures sont conçues manuellement selon des règles algorithmiques bien établies, avec des garanties sur leur comportement même dans le pire des cas.

Les **learned indexes** (index appris) constituent une approche innovante qui remplace partiellement ou totalement ces structures par des modèles de machine learning, capables d’apprendre la distribution des données et ainsi prédire la position d’une clé plus efficacement.

Le concept a été popularisé par l’article seminal *The Case for Learned Index Structures* (Kraska et al., 2018) qui a montré que l’apprentissage de la distribution des données permettait de construire des index souvent plus compacts et rapides que les B-trees traditionnels.

### Principe de fonctionnement

- Le learned index est une **fonction de prédiction** formée par apprentissage supervisé sur les données présentes dans la table.  
- Cette fonction, souvent un modèle de régression, estime la position ou le rang d’un élément clé donné dans la structure sous-jacente ordonnée.  
- L’algorithme commence par une approximation rapide de la position grâce au modèle appris, puis effectue un raffinement (recherche locale) pour trouver la clé exacte.

---

## Usage en bases de données

### Avantages principaux

- **Gain en vitesse** : Prédiction directe de l’emplacement probable d’une clé, réduisant le nombre d’accès mémoire.  
- **Réduction de l’espace** : Moins de structures auxiliaires à stocker par rapport à un arbre complet.  
- **Adaptabilité à la distribution des données** : Le modèle s’adapte naturellement aux données non uniformes.

### Cas d’application

- Indexation de grandes tables très dynamiques dans les bases relationnelles.  
- Systèmes de recherche haute performance nécessitant de minimiser la latence d’accès.  
- Bases NoSQL avec gros volumes de données structurées.

### Exemples

| Traditionnel    | Learned index                          | Commentaire                 |
|-----------------|--------------------------------------|-----------------------------|
| B-tree          | Modèle de régression (ex : RMI)      | Moins d'étages, stockage compact |
| Hashing classique| Réseau de neurones apprenant la clé | S’adapte aux données non uniformes |

---

## Illustration Mermaid

```mermaid
graph LR
    A[Clé recherchée] --> B[Modèle appris]
    B --> C[Prédiction position approximative]
    C --> D[Recherche locale (ex: interpolation, Binaire)]
    D --> E[Position exacte clé]
```

Ce flux montre comment un learned index s’appuie sur un modèle prédictif pour accélérer la recherche.

---

## Limites et défis

- **Coût d’entraînement** : Modélisation et mise à jour du modèle peuvent introduire un surcoût en environnements très dynamiques.  
- **Robustesse** : En cas de changements brutaux dans la distribution des données, le modèle peut devenir inefficace.  
- **Sécurité** : Des attaques adversariales peuvent cibler le modèle (voir séance suivante), compromettant l’efficacité de l’index.

---

## Sources et références

- Kraska, Tim, et al. "The Case for Learned Index Structures." Proceedings of the 2018 ACM SIGMOD International Conference on Management of Data. ACM, 2018.  
- Marcus, Robert, et al. "Benchmarking learned indexes." Proceedings of the 2019 International Conference on Management of Data. ACM, 2019.  
- [Learned Indexes - A primer by CMU](https://lamastex.github.io/learned-indexes/)  
- [Database Indexing: Traditional vs Learned - Medium article](https://medium.com/@dmitryalexeev/db-index-traditional-vs-learned-6e0e2ed962e1)  

---

Cet article présente de manière concise le concept des learned indexes en bases de données, expliquant leur mode de fonctionnement par apprentissage, leurs avantages pratiques ainsi que les enjeux liés à leur déploiement. Ces modèles ouvrent la voie à une nouvelle génération d’index adaptatifs et performants, exploités dans les systèmes modernes de gestion de données.