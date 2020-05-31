---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Eplus Wrapper"
summary: "Energy⁺ wrapper est une librairie permettant d'interfacer Energy⁺ à des scripts python."
authors: []
tags: ["wrapper", "simulation thermique dynamique", "thermique"]
categories: []
date: 2020-05-24T12:13:48+02:00

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

url_code: "https://github.com/locie/energy_plus_wrapper"
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---
Energy Plus est un logiciel de simulation dynamique du bâtiment, capable de prendre en compte les phénomènes thermique, hydrique et aérolique prenant place dans un habitat. Il donne accès à une résolution temporelle à l'échelle de quelques jours, mois ou même années. C'est un outil précieux permettant d'affiner notre compréhension du comportement des bâtiments, d'en faire des prédiction ou de le caractériser (via résolution inverses, analyse de sensibilité...).

Ces dernières méthodes nécessitent un nombre plus ou moins importants de simulations, effectués en changeant un ensemble restreint de paramètres.

Malheureusement, Energy Plus n'a pas été construit dans cette optique, et des précautions doivent être prises dès lors qu'il est nécessaire de lancer plusieurs simulations en parallèles.

Energy⁺ wrapper est une librairie permettant d'interfacer Energy⁺ à des scripts python. De cette façon, il est facile de lancer un nombre important de simulations, de récupérer et d'analyser les résultats obtenus. Elle a été écrite de façon à être multiprocess-safe, et la résolution de multiples simulations en parallèle est directement intégré à l'outil.

Sa [dernière version](https://github.com/locie/energy_plus_wrapper/tree/full_rework) est une refonte complète, amenant (entre autre) une gestion intégré du lancement de simulation en parallèle via la librairie [joblib]. Cela donne accès à un choix élargie de backend de parallélisation incluant un backend de calcul distribué : il est alors possible, en quelques ligne de code, de lancer les calculs sur différentes machine d'un même réseau.