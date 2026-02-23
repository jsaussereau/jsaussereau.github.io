---
title: "Installer Odatix à l'ENSEIRB"
date: "2026-02-17T00:00:00+02:00"
summary: "Tutoriel d'installation d'Odatix à l'ENSEIRB-MATMECA"
weight: 3
tags:
  - "ENSEIRB-MATMECA"
  - "Electronique"
  - "SEE"
  - "Tuto"
menu:
  sidebar:
    name: "Installer Odatix à l'ENSEIRB"
    parent: "asic_tools"
    identifier: "install_odatix"
    weight: 3
hero: install_odatix-large.png
mermaid: true
draft: false
hidden: false
---

Odatix est un outil conçu au Laboratoire IMS pour faciliter l'implémentation et la validation de designs numériques configurables à travers de divers outils FPGA et ASIC (dont Vivado et Design Compiler).

Odatix permet de :
  - Explorer différentes configurations architecturales.
  - Trouver automatiquement la fréquence maximale de fonctionnement.
  - Lancer des synthèses logiques en parallèle à n'importe quelles fréquences ou plage de fréquence.
  - Simuler en parallèle pour valider et évaluer les performances.
  - Visualiser les résultats dans une interface interactive.

# Installation

> [!INFO]
> Ces étapes ne sont à réaliser qu'une seule fois.

## Installation via pip

Dans un terminal :

```bash
pip install odatix
```

## Ajout des programmes python au `PATH`

> [!NOTE]
> Par défaut, à l'ENSEIRB, le dossier `$HOME/.local/bin` (où s'installent les applications installées via pip) n'est pas dans la variable d'environnement `PATH`, il faut donc l'ajouter pour pouvoir exécuter Odatix depuis n'importe quel répertoire.

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> $HOME/.bashrc
```

## Ajoute de Design Compiler au `PATH`

Pour faciliter l'utilisation de Design Compiler et permettre à Odatix de l'appeler, nous allons aussi ajouter à la variable d'environnement `PATH` le chemin vers les binaires de Synopsys Design Compiler, ainsi que la variable d'environnement nécessaire pour la licence :


```bash
echo 'export SNPSLMD_LICENSE_FILE=27000@synopsys.cnfm.fr' >> $HOME/.bashrc
```

```bash
echo 'export PATH="/opt/synopsys/design_vision/syn/H-2013.03-SP5/bin:$PATH"' >> $HOME/.bashrc
```

## Rechargemment du shell

Rechargez ensuite votre fichier `.bashrc` pour appliquer les modifications :

```bash
source $HOME/.bashrc
```
