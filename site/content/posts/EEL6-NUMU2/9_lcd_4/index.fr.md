---
title: "09. LCD Etape #4"
date: "2026-02-11T00:00:00+02:00"
summary: "Développemment de la bibliothèque LCD - étape 4 : développement de la fonction d'initialisation."
menu:
  sidebar:
    name: "LCD Etape #4"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-lcd_4"
    weight: 9
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-lcd_etape4.png"
mermaid: true
draft: false
hidden: false
---

### <ins>Étape 4</ins> : Développement de la fonction d'initialisation
La page 11 de la datasheet du module LCD *DS_Afficheurs_Sunplus* détaille la procédure d'initialisation du module.

> [!TIP]
> Plutôt que d'envoyer les commandes avec les données brutes dans cette procédure, il est préférable de comprendre ce que fait chacune d'entre elles.
> Ainsi, on remarque qu'une grande partie de la procédure d'initialisation peut être réalisée en effectuant des appels aux fonctions définies plus haut.

> [!IMPORTANT]
> - Penser à l'alimentation du module (*cf.* datasheet de la carte PICDEM2+).
> - Penser aux ports du microcontrôleur qui ont été utilisés... Ont-ils bien été définis comme entrée/sortie ?

> [!NOTE]
> À ce stade on peut tester si tout fonctionne. Pour cela, on peut ajouter un appel à `lcd_display_control` avec les bons paramètres pour allumer l'écran et faire clignoter le curseur.
> Ainsi, si après avoir téléversé le programme sur la carte, le curseur clignote sur l'écran, les étapes précédentes sont validées !
