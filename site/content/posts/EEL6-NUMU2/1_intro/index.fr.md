---
title: "01. Introduction"
date: "2026-02-11T00:00:00+02:00"
description: "Cette section détaille comment obtenir les fichiers nécéssaires au déroulement du projet et décrit l'organisation du répertoire."
menu:
  sidebar:
    name: "Introduction"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-intro"
    weight: 1
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-intro.png"
mermaid: true
draft: false
hidden: false
---

## Télécharger l'archive

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

