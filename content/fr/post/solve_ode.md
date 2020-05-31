---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Introduction à la résolution d'équations différentielles ordinaires (ODE)"
subtitle: ""
summary: ""
authors: ["celliern"]
tags: ["ode", "calcul numérique"]
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

Nous allons prendre un exemple très simple : [l'évolution d'un pendule simple.](https://fr.wikipedia.org/wiki/Pendule_simple#Mise_en_%C3%A9quation)

Il est possible d'écrire l'évolution de l'angle entre la masse $m$ situé à une distance $l$ de son point d'accroche:

$$\ddot\theta(t) + \frac{g}{l} \sin{\theta(t)}$$

qu'on écrit comme un système de deux équations du premier ordre en introduisant la vitesse angulaire $\omega = \dot\theta(t)$ :

\begin{align}
  \dot\omega(t) &= -\frac{g}{l} \sin{\theta(t)} \\\\
  \dot\theta(t) &= \omega(t)
\end{align}

La façon la plus simple de résoudre un système de ce genre est de remplacer les dérivés temporelles par une approximation déduite de la [formule de Taylor-Young](https://fr.wikipedia.org/wiki/Formule_de_Taylor-Young).

$$\theta(t + \Delta t) = \theta(t) + \Delta t\\,\dot\theta(t) + \mathcal{O}(\Delta t)$$

Soit

$$\dot\theta(t) \approx \frac{\theta(t + \Delta t) - \theta(t)}{\Delta t}$$

ou, en remplaçant utilisant une notation plus compacte

$$\dot\theta(t) \approx \frac{\theta^{n+1} - \theta^{n}}{\Delta t}$$

{{% alert note %}}

Le terme $\mathcal{O}(\Delta t)$ indique que l'approximation est exacte à facteur $\mathcal{O}(\Delta t)$ près, facteur du même ordre de grandeur que $\Delta t$ : il diminuera "à la même vitesse" que $\Delta t$.

{{% /alert %}}

En remplaçant les dérivés par leur approximation, on obtient

\begin{align}
  \frac{\omega^{n+1} - \omega^{n}}{\Delta t} &= -\frac{g}{l} \sin{\theta^{n}} \\\\
  \frac{\theta^{n+1} - \theta^{n}}{\Delta t} &= \omega^{n}
\end{align}

ce qui une fois réorganisé donne

\begin{align}
  \omega^{n+1} &= \omega^{n} - \Delta t \frac{g}{l} \sin{\theta^{n}}\\\\
  \theta^{n+1} &= \theta^{n} + \Delta t \omega^{n}
\end{align}

On a alors un système d'équation permettant de calculer nos variables dépendantes pour le prochain pas de temps en fonction de leur valeurs au temps actuel. On comprend également assez bien la nécessité d'une condition initiale pour fermer le système !

Ce schéma est est un schéma dit *explicite* (c'est le schéma Euler explicite) car il est possible de calculer directement l'état du système au pas suivant à partir de grandeurs connus à priori.

On peut généraliser ce travail à n'importe quel système d'équation en l'écrivant sous forme vectorielle :

\begin{align}
  \dot U = F(U)
\end{align}

avec dans notre cas $$U = [\omega(t), \theta(t)]$$

On obtient alors

$$
  U^{n+1} =  U^{n} + \Delta t\\,F(U^{n})
$$

$F(U)$ est appelé $vecteur d'évolution$ du système.

La résolution est alors très simple, ici un exemple en Julia:

```julia
l = 1e-2; g = 9.81
t = 0; θ = deg2rad(30); ω = 0
Δt = 1e-6
while t < 1
    dω = - g / l * sin(θ); dθ = ω
    ω += Δt * dω; θ += Δt * dθ
    t += Δt
end
```

![pendule simple](/img/ode_pde/pendule_01.svg)

Mais comment évolue le résultat quand le pas de temps est modifié? Avec un pas de temps $\Delta t = $

![pendule simple, dt variable](/img/ode_pde/pendule_02.svg)

On observe une augmentation de l'amplitude de l'oscillation du pendule, alors qu'elle devrait rester constante au cours du temps. Je ne ferais pas la démonstration ici, mais cela illustre une caractéristique des schémas explicites : ils ajoutent de l'énergie de façon artificielle au système.

**C'est une leçon importante: le choix du schéma numérique et du pas de temps à un impact qui peut être dramatique sur les résultats d'une modélisation**

## Méthode explicite / implicite

Un autre point important des schéma explicites sont leurs instabilités intrinsèque. Une analyse de stabilité montre que ces schéma sont *conditionnellement stables*, et qu'il existe d'un pas de temps critique $t_c$ au dela duquel le schéma devient instable.

Il existe d'autres schéma numériques qui sont *inconditionnellement stables* : dans ce cas, il sera possible de prendre des pas de temps beaucoup plus grands. Attention tout de même: ce n'est pas parce que le schéma est stable qu'il sera précis !

Les développements limités permettent également d'obtenir le schéma Euler **implicite** qui s'écrit

$$
  \frac{U^{n+1} - U^{n}}{\Delta t} =  F(U^{n+1}) + \mathcal{O}(\Delta t)
$$

On comprend alors le terme *implicite* : il n'est pas toujours possible de résoudre cette équation (à l'exception notable des systèmes linéaires). Il est toutefois possible de linéariser l'équation en écrivant

$$
  \frac{U^{n+1} - U^{n}}{\Delta t} =  F(U^{n}) + J(U^{n}) \\, \left(U^{n + 1} - U^{n}\right)
$$

avec $J(U)$ la [matrice jacobienne](https://fr.wikipedia.org/wiki/Matrice_jacobienne) de $F$.

Au dela de leurs stabilités, les schéma implicites ont aussi tendance à diminuer artificiellement l'énergie des systèmes modélisés (l'effet inverse des schéma explicites).

Il existe un grand nombres de schéma capables de résoudre une équation différentielle ordinaire.