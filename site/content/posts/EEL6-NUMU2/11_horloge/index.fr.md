---
title: "11. Affichage de l'horloge"
date: "2026-02-11T00:00:00+02:00"
description: ""
menu:
  sidebar:
    name: "Affichage de l'horloge"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-horloge"
    weight: 11
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-horloge.png"
mermaid: true
draft: false
hidden: false
---

<h2 id="aide_horloge"> 4. Affichage de l'horloge sur l'écran LCD </h2>

Maintenant que l'on peut afficher ce que l'on veut sur écran, on cherche à afficher l'horloge.
On utilise donc la fonction d'interruption (déclenchée toutes les secondes grâce à la configuration du timer) pour générer les heures, minutes et secondes qui seront affichés sur l'écran.

> [!TIP]
> Comme toujours en programmation microcontroleur, bien réfléchir à ce qui doit être fait dans la fonction d'interruption et ce qui doit être fait ailleurs...

Pour formatter l'horloge dans une chaîne de caractère, le plus simple est certainement d'utiliser la fonction `sprintf` de la bibliothèque `stdio`.
Pour obtenir la documentation de `sprintf`, il suffit de taper dans un terminal :
```bash
man sprintf
```
On voit qu'elle fonctionne comme `printf` mais avec un argument en plus : une chaîne de charactère de destination.

Exemple :
```c
char formatted_time[STRING_LENGTH]; // STRING_LENGTH: nombre de caractères après formatage + 1 (pour le '\0' de fin de chaîne)
sprintf(formatted_time, "%d:%d:%d", t.hours, t.minutes, t.seconds);
```
Résultat : `12:1:8`

Comme `printf`, on peut forcer une mise en forme sur un nombre précis de charactères :
```c
sprintf(formatted_time, "%2d:%2d:%2d", t.hours, t.minutes, t.seconds);
```
Résultat : `12: 1: 8`

De même, on peut forcer l'affichage des 0 :
```c
sprintf(formatted_time, "%02d:%02d:%02d", t.hours, t.minutes, t.seconds);
```
Résultat : `12:01:08`
