---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Kata - SRP
info: |
  ## Kata rover on SRP
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
---

# Code Kata

Single Responsability Principal

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Are you ready ?<carbon:arrow-right />
</div>


<!--
Partager la MR du repo kata pour la séance

https://gitlabee.dt.renault.com/irn-71028/kata/rover/-/merge_requests/1
-->

---
transition: fade-out
---

# Sommaire

<Toc minDepth="1" maxDepth="1" />

<!--
-->
---
transition: fade-out
---

# Plan de la séance

Etapes
- Introduction 5 min
- Lecture du code "naîf" 10 min
- Identification des responsabilités 10 min
- Refactoring guidé 30 min
- Tests et validation 10 min
- Debrief & Retours 10 min
<br>

<v-click>
Supports

- Fiche SRP
- Schéma SRP Rover
- Slides
- Code Java
</v-click>


<!--
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
---
transition: fade-out
---

# Rappel - Mars Kata Rover

Le Rover doit explorer une grille en obéissant à des commandes :
- M : avancer
- L : tourner à gauche
- R : tourner à droite

Il a une position de départ ainsi qu'une orientation E, N, W, S

<br>

<v-click>
Aujourd'hui, on va surtout...

➡ Refactorer pour faire émerger Single Responsibility Principle (SRP).

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>


<!--
Expliquer prendre max 10min


Introduire le principes du SRP 


Travail collectif
-->

---
transition: fade-out
layout: image-right
image: /grid.jpg
dragPos:
  square: 861,31,70,62,265
---

# Exemple

Position de départ: (2,1) 


Il regarde vers W


Commandes: RMMLMM

<br>


<v-click>Où se trouve le rover ?</v-click>


<br>


<v-click>(0,3) regardant vers W</v-click>

<img v-drag="'square'" src="/rover.png" style="Height: 20%">

---
transition: fade-out
---

# Single Responsibility Principle

Le SRP pour les intimes ...

C'est quoi pour vous ?

<br>

<v-click>
Une classe (ou méthode) doit avoir <span v-mark.orange="1">une et une seule responsabilité.</span>

Elle ne devrait avoir <span v-mark.orange="1">qu'une seule raison de changer.</span>
</v-click>

<br>
<v-click>

Comment détecter un problème de SRP ?

</v-click>



<v-click>

- Trop de <span v-mark.circle.orange="2">"ET"</span> dans la description	Exemple : "Cette classe gère la position ET la direction ET les commandes."

</v-click>
<v-click>

- Les méthodes ont des noms <span v-mark.orange="3">longs ou flous</span> :	Exemple: "executeCommandsAndMoveAndTurn"

</v-click>
<v-click>

- Des changements <span v-mark.circle.orange="4">fréquents</span> dans des zones non liées :	Exemple: Ajout d'un type de mouvement → casse la gestion de position.

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Attendu -> Une classe (ou méthode) doit avoir une et une seule responsabilité. Elle ne devrait avoir qu'une seule raison de changer.

2. Comment détecter une violation SRP 2min

Trop de "ET" dans la description	Exemple : "Cette classe gère la position ET la direction ET les commandes."

Les méthodes ont des noms longs ou flous :	Exemple: "executeCommandsAndMoveAndTurn"

Des changements fréquents dans des zones non liées :	Exemple: Ajout d'un type de mouvement → casse la gestion de position.

-->

---
transition: fade-out
---

# Code de départ brut et naïf

Rover gère

<div v-click-hide>Parcourons ensemble le code</div>

<div v-after>


  - Commandes
  - Mouvement
  - Direction
  - Affichage
</div>

<v-click>

Identitifons ensemble les différentes responsabilitées, ainsi que les raisons de pourquoi est ce que ce bout de code devrait changer 

</v-click>

<v-click>

|            |                        |
|------------|------------------------|
| Commandes  | Ajouter commande       |
| Direction  | Modifier rotations     |
| Position   | Modifier déplacements  |


</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Commencer par parcourir le code de départ

Dire ce que fait le rover:

Commandes, mouvement, direction, affichage

En extraire des responsabilités

Commandes, direction, position
-->

---
transition: fade-out
layout: two-cols
---

# Refacto vers le modèle émergeant du SRP


1. Extraire Direction
2. Extraire Position
3. Réduire Rover
4. Extraire CommandInterpreter

<br> 


::right::

```mermaid {theme: 'neutral', scale: 0.75}
---
title: Modèle émergeant SRP
---
classDiagram

  CommandInterpreter <|-- Rover
  Rover <|-- Position 
  Rover <|-- Direction  

  class CommandInterpreter{
    + execute(rover, cmds) 
  }
  class Rover {
    + move()  
+ turnLeft()
+ turnRight()
  }

  class Position {
+int x
+int y
  }

  class Direction {
    Enum: N, E, S, W

  }

```

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Laisser afficher pendant le refacto

Faire le tour du modèle vérifier que c'est bon

Lancer la séance solo max 30min
-->

---
transition: fade-out
---

# Let's code !


<img src="/coding.png" style="display: block; margin: 0 auto; width: 75%;"/>

---
transition: fade-out
layout: center
---

# Pour aller plus loin


Pour aller plus loin:

- Implémenter la gestion d'obstacles
- Faire une carte circulaire
- Ajouter des logs de déplacement

---
transition: fade-out
layout: end
---



# Conclusion
Debrief et retours

<br>

Où voyez-vous les bénéfices immédiats ?


Où appliquer ces principes dans vos projets ?

<img src="/kata.jpg" alt="Kata" style="display: block; margin: 0 auto; width: 50%;"/>
<!--
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Laissez parler
-->
