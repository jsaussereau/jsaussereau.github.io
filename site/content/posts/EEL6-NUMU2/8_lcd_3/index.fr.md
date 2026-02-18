---
title: "08. LCD Etape #3"
date: "2026-02-11T00:00:00+02:00"
description: ""
menu:
  sidebar:
    name: "LCD Etape #3"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-lcd_3"
    weight: 8
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-lcd_etape3.png"
mermaid: true
draft: false
hidden: false
---


### <ins>Étape 3</ins> : Développement des fonctions correspondant aux différentes commandes
La page 7 de la datasheet du module LCD *DS_Afficheurs_Sunplus* liste toutes les commandes proposées par le module. Avant de chercher à développer la fonction d'initialisation, il est préférable de développer des fonctions correspondant à ces commandes.

Notamment les suivantes :

- `Clear Display `
- `Return Home ` 
- `Entry Mode Set `
- `Display ON/OFF Control `
- `Function Set `
  
> [!TIP]
> Ces commandes sont caractérisées par 3 valeurs : `rs`, `rw` et une donnée de 8 bits.
> Ce sont justement les paramètres de la foncton `lcd_write_instr_8bits` décrite plus tôt.

> [!TIP]
> Pour générer la donnée de la commande avec les bons arguments, il sera nécessaire pour certaines fonctions d'effectuer des [opérations binaires](https://dept-info.labri.fr/ENSEIGNEMENT/programmation1/cours/CM_9___Manipulation_binaire.pdf).

