---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Équations différentielles, qu'est-ce que c'est?"
subtitle: "Petite introdution aux systèmes d'équations différentielles ordinaires / aux dérivés partielles"
summary: ""
authors: ["celliern"]
tags: ["pde", "pde", "calcul numérique"]
categories: []
date: 2020-05-31T12:54:04+02:00
lastmod: 2020-05-31T12:54:04+02:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: ["scikit-fdiff"]
---

Très souvent lorsque l'on veut modéliser un phénomènes physique via une équation mathématique, nous n'avons pas accès à une équation représentant la grandeur physique au cours du temps mais à une description de comment elle va évoluer. Si nous souhaitons connaitre la température au sein d'un mur, il serait appréciable d'avoir une forme du genre

$$T(t, x, y, z) = ...$$.

{{% alert note %}}
Nous parlerons de variables dépendante pour les grandeurs inconnues dont nous voulons calculer l'évolution (ici la température $T$) et de variables indépendantes (ici $t$, $x$, $y$, $z$) dont dépendent les précédentes.
{{% /alert %}}

Malheureusement, ce n'est que très rarement le cas : l'évolution de la température va dépendre de beaucoup trop d'éléments (la forme du mur, de la température à un instant $t\_0$, des conditions *extérieur*, c'est à dire au delà des limites de notre modèle). À la place, nous avons accès à une description de l'évolution de la température sous forme d'une équation aux dérivés partielles (EDP, ou PDE en anglais).

Si le modèle ne dépend que du temps (on parle parfois de modèle 0D), on tombe sur une (ou un système de) équation différentielle ordinaire (EDO ou ODE en anglais). La modélisation d'un lancer de balle de golfe (ou d'un boulet de canon), celle d'un pendule simple ou double mèneront à des systèmes d'EDO. En plus du modèles et de ses paramètres, il faut ajouter une condition initiale pour fermer le problème et calculer l'évolution de la grandeur inconnue. Dans le cas d'un tir au canon, il faudra connaitre la position et la vélocité initiale du boulet. Si cette information est inconnu, il existe une infinité de solution possible.

Dans le cas contraire, que le modèle dépend de plusieurs variables indépendantes, il faudra également ajouter des conditions limites, et savoir comment se comporte la grandeur aux frontières du domaine d'étude. Dans le cas d'un mur, nous pouvons indiquer que la température à ses frontières est fixe, ou qu'un certain flux de chaleur est imposé.

Revenons au cas de l'évolution de la température dans un mur. Pour connaître son évolution au cours du temps, nous allons utiliser l'équation de la chaleur. Celle ci se déduit de l'utilisation des lois élémentaires de la thermodynamique et peut se retrouver en faisant un bilan d'énergie dans un volume de contrôle de taille *infinitésimal*, dont les dimensions tendent vers 0.

Si on se limite à un cas simple et que l'on considère une seule dimension, l'équation obtenu est la suivante :

$$\rho Cp\ \partial\_t T = \lambda \partial_{xx} T$$

{{% alert note %}}

J'utilise la notation $$\partial_{x} U = \frac{\partial U}{\partial {x}}$$ pour alléger l'écriture des équations. On retrouve d'autres notations pour décrire les dérivées partielles comme par exemple 

$$U_{x} = \frac{\partial U}{\partial {x}}$$ chez certains auteurs : toutes ces notations ont exactement le même sens.

{{% /alert %}}

Il est parfois possible de résoudre ces équations de façon analytique. C'est toutefois **très** rare, et uniquement dans des cas très spécifiques. Dans le cas contraire, il va être nécessaire d'utiliser des méthodes de résolution numériques.

Parmis ces méthodes, on retrouve la méthode des différences finis, la méthode des volumes finis, des éléments finis, les méthodes pseudo-spectrales... et j'en passe. A noter que les stratégies d'approximation des dérivées spatiales et temporelles peuvent être différentes : c'est le propre de la méthode des lignes.

Dans ce cas, nous allons dans un premier temps discrétiser les dérivées spatiales de façon à passer d'un système du type

$$
  \partial_t u = F(u, \partial_x u, \partial_y u, \partial_{xx} u, \partial_{yy} u...)
$$

à un système du type

$$
  \partial_t U_{ij} = F(U_{i-1,j-1}, U_{i-1,j}, U_{i-1,j+1}, U_{i,j-1}, U_{i,j+1}...)
$$

avec $U_{ij}(t)$ les valeurs de $U$ aux nœuds d'une grille discrète maillant le domaine de résolution.

Une fois cette discrétisation effectuée, l'équation aux dérivés partielle est devenu un (gros) système d'équations différentielles ordinaires. il est alors possible d'appliquer les algorithme de résolution d'ODE.