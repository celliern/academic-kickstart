---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Scikit Fdiff"
summary: "`scikit-fdiff` est une librairie Python dont l'objectif est de rendre accessible la résolution d'équations aux dérivées partielles."
authors: []
tags: ["résolution d'EDP"]
categories: []
date: 2020-05-24T12:13:25+02:00

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: "https://gitlab.com/celliern/scikit-fdiff/"
url_pdf: "https://joss.theoj.org/papers/10.21105/joss.01356"
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

`scikit-fdiff` est une librairie Python dont l'objectif est de rendre accessible la résolution d'équations aux dérivées partielles.

Son développement à commencé durant mon projet de doctorat (2016), et la librairie s'appelait alors `triflow`. C'était alors un outil interne dédié à la résolution d'équations type Shallow-Water utilisé pour modéliser des films ruisselants.

Au fur et à mesure de son développement, son champs d'application s'est élargie pour devenir un outil permettant la résolution de système de PDE arbitraires, capable de modéliser un nombre arbitraire d'équations et de dimensions.

L'outil se fonde sur la méthode des lignes, dans laquelle les dérivées spatiales sont discrétisés dans un premier temps. Le système d'équations différentiels ordinaire alors obtenu est résolu en utilisant des algorithmes dédiés, dont les plus connus sont les méthodes d'Euler ou les méthodes Runge-Kutta.

La spécificité de `scikit-fdiff` est d'automatiser la discrétisation spatiale, permettant une écriture du modèle sous une forme aussi proche que possible de l'écriture mathématique du modèle physique. Le schéma de discrétisation (en différence fini) est automatiquement appliqué via calcul formel. La jacobienne est calculé analytiquement, donnant accès aux méthodes implicites. Cela permet la résolution de modèles fortement non-linéaires.

Ces caractéristiques font de `scikit-fdiff` une librairie particulièrement adapté au prototypage de modèles : s'il sera rarement le meilleur outil pour un modèle en particulier, il sera capable de gérer une grande majorité de systèmes d'équations.

Le développement est actif, et la librairie a été publié dans JOSS (Journal of open source software) [^CellierJoss2019].