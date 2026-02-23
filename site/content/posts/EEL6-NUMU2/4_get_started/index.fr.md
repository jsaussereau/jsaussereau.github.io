---
title: "4. Get started"
date: "2026-02-11T00:00:00+02:00"
summary: "Téléchargement des fichiers nécessaires au déroulement du projet et description de l'organisation du répertoire."
weight: 4
menu:
  sidebar:
    name: "Get started"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-get_started"
    weight: 4
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-get_started.png"
mermaid: true
draft: false
hidden: false
---

<!-- <h2 id="aide_get_started"> 1. Get started 🏁</h2> -->

## Téléchargemment du répertoire

- Exécuter dans un terminal :

```bash
cd ~/Documents/
git clone https://github.com/jsaussereau/EN111PR.git
```

- Le dossier `EN111PR` devrait être apparu sur votre session dans le dossier `Documents`

<details>
<summary>Solution alternative</summary>

- Cliquer [ici](https://github.com/jsaussereau/EN111PR/archive/refs/heads/main.zip) pour télécharger l'archive.
- Extraire l'archive sur la session dans **Documents**.

</details>

## Organisation du répertoire
Il y a 3 dossiers principaux :
* **doc** : Contient les documents nécessaires pour le projet : sujet, datasheet du PIC16F877A, datasheets de la carte et de l'afficheur.
* **src** : Contient les fichiers sources à utiliser (déjà importés dans le projet).
* **work** : Le dossier dans lequel se situe le projet MPLABX, déjà configuré.

## Démarrage du projet

Un projet déjà configuré est disponible dans `work`. Pour l'ouvrir :

1. Dans MPLABX, aller dans **File > Open Project**.
2. Sélectionner le projet **PROJET.X** présent dans le dossier `~/Documents/EN111PR/work`.

Plusieurs fichiers sont déjà créés dans le dossier `src` (voir "Header Files" et "Source Files" dans MPLABX) :
- `main.c` : Fichier principal. C'est ici que se trouve la fonction main et la fonction d'interruption.
- `timer.c` et `timer.h` : Fichiers où développer [la configuration du timer](/fr/posts/eel6-numu2/5_led/), [la mise en forme](/fr/posts/eel6-numu2/11_horloge/) et [la configuration](/fr/posts/eel6-numu2/12_configuration/) de l'horloge.
- `lib_LCD.c` et `lib_LCD.h` : Fichiers où développer [la bibliothèque LCD](/fr/posts/eel6-numu2/6_lcd_1/).
	
> [!WARNING]
> Pensez à **faire valider votre travail à chaque partie**, avant de passer aux étapes suivantes.
