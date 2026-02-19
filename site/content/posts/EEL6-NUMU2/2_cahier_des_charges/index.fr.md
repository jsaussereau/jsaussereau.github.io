---
title: "02. Cahier des charges"
date: "2026-02-11T00:00:00+02:00"
summary: "Liste des objectifs principaux (obligatoires) et secondaires (bonus) du projet."
menu:
  sidebar:
    name: "Cahier des charges"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-cahier_des_charges"
    weight: 2
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-cahier_des_charges.png"
mermaid: true
draft: false
hidden: false
---

## Objectifs principaux
- Faire clignoter une des LED à la fréquence 0.5 Hz (1 changement d'état toutes les secondes) à partir d'**interruptions** sur le Timer1. Cette fonctionnalité doit être maintenue même après le développement des étapes suivantes. ([Aide](/fr/posts/eel6-numu2/5_led/))

- Développer une bibliothèque pour l'afficheur LCD (`lib_LCD.h` et `lib_LCD.c`) proposant à l'utilisateur au minimum les fonctionnalités suivantes ([Aide](/fr/posts/eel6-numu2/6_lcd_1/)) :
	- `lcd_init` : Initialisation générale de l'afficheur en mode 4 bits
	- `lcd_putch` : Ecriture d'un caractère sur l'afficheur
	- `lcd_puts` : Ecriture d'une chaîne de caractères sur l'afficheur
	- `lcd_pos` : Positionnement du curseur en (x,y) - origine en (1,1)
	- `lcd_home` : Cursor home : positionnement du curseur (1,1)
	- `lcd_clear` : Effacement de l'écran et cursor home  
	<br />
- Affichage sur l'écran LCD d'une horloge au format `HH:MM:SS` qui s'incrémente à chaque seconde (via le Timer1). ([Aide](/fr/posts/eel6-numu2/11_horloge/))
- Pouvoir configurer l'horloge à l'aide des boutons poussoir S2 et S3 ([Aide](/fr/posts/eel6-numu2/12_configuration/)) :
	- Un appui prolongé d'au moins 2 s sur S2 fera clignoter les heures, celles-ci s'incrémenteront à chaque appui sur S3, ou automatiquement (f ≈ 5 Hz) en cas d'appui maintenu au-delà de 2 s.
	- Un nouvel appui sur S2 permettra un réglage des minutes selon la même procédure.
	- Éventuellement, un nouvel appui sur S2 permettra un réglage des secondes selon la même procédure.
	- Un dernier appui sur S2 fera quitter le mode "réglage".  

## Objectifs Secondaires
#### (Au choix et si le temps le permet)
- Utilisation du potentiomètre pour régler les heures/minutes/secondes.
- Ajout de caractères personnalisés sur l'afficheur.
- Récupération via le protocole I²C de la température du capteur TC74.
- Ajout d'une alarme quand l'horloge atteint une heure configurée, avec une sonnerie ou une mélodie sur le buzzer P1.
- Utilisation de l'écran comme un afficheur à décalage (comme dans les bus/trams).
- Toute autre idée que vous aimeriez développer avec les éléments à disposition sur la carte.

