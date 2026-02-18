---
title: "03. Evaluation"
date: "2026-02-11T00:00:00+02:00"
description: ""
menu:
  sidebar:
    name: "Evaluation"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-evaluation"
    weight: 3
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-evaluation.png"
mermaid: true
draft: false
hidden: false
---


## Modalités de rendu

Le rendu se fait par mail avec comme objet `[EN111] NOM1 NOM2`, avant le 25 mai 23:59.

- Groupes D et E : [nader.ajmi@bordeaux-inp.fr](mailto:nader.ajmi@bordeaux-inp.fr?subject=[EN111]%20NOM1%20NOM2)
- Groupe F : [valery.lebret@enseirb-matmeca.fr](mailto:Valery.Lebret@enseirb-matmeca.fr?subject=[EN111]%20NOM1%20NOM2)
- Groupe G : [jsaussereau@bordeaux-inp.fr](mailto:jsaussereau@bordeaux-inp.fr?subject=[EN111]%20NOM1%20NOM2)


## Ressouces à rendre

- Le code source (tous les fichiers `.c` et `.h` dans le dossier `src`) compressés dans une archive `.zip`
- Un rapport par binôme, d'environ 10 pages (hors annexe), au format `.pdf`, contenant :
	- Une introduction du contexte en résumant le cahier des charges et en présentant les ressources utiles de la carte.
	- Une explication de la conception de chacune des parties du projet :
		- Génération de l'interruption toutes les secondes
			- Configuration du timer
			- Configuration du module CCP
		- Développement de la bibliothèque pour l'afficheur LCD 
			- Simplification des accès
			- Développement d'une fonction d'envoi de n'importe quelle commande
			- Développement des fonctions correspondants aux différentes commandes
			- Développement de la fonction d'initialisation
			- Développement des fonctions utilisateur restantes
		- Affichage de l'horloge sur l'écran LCD 
		- Développement de la fonctionnalité de configuration de l'horloge
		- Eventuelle(s) partie(s) bonus
	- Une conclusion sur les enseignements que l'on peut tirer de ce projet. Tant d'un point de vue technique que méthodologique.

## Compléments sur le rapport

- Dès que l'on utilise **directement** des registres du microcontrôleur, expliquer : 
	- Quels sont les registres utilisés ?
	- Quelles sont les valeurs qui ont été mises dans ces registres ?
	- Quelles actions ont ces valeurs techniquement ? 
	- Quelles sont les fonctionnalités recherchées qui justifient ces valeurs ?
- Pour l'explication des fonctions, il ne s'agit pas de juste expliquer ce qu'elles font fonctionnemment (ça c'est le cahier des charges, que l'on a déjà). Il s'agit d'expliquer **comment elles ont été implémentées** (opérations utilisées, logique, optimisations, ...) et pourquoi elles sont implémentées comme ça et pas autrement.
- De manière générale, dès que des choix on été fait, comme des choix de configuration de module, ou des choix d'implémentation, expliquer ces choix, **même quand plusieurs configurations correspondaient aux exigences**. 
- La description de `lcd_write_instr_4bits`, `lcd_write_instr_8bits` et `lcd_init` pourra être accompagnée d'organigrammes expliquant le déroulement de ces fonctions.
- La description de la partie de configuration de l'horloge pourra être accompagnée d'un diagramme de la machine d'état.
- Bien faire apparaître, et justifier, ce qui est effectué par interruption et ce qui ne l'est pas.
- Même si vous n'avez pas réalisé une partie, vous pouvez l'indiquer et expliquer comment vous auriez fait.
- Bon courage ;)

> [!IMPORTANT]  
> 1. Le code source doit compiler !
> 2. La note qui vous sera attribuée sur ce module tient également compte du travail observé durant les séances de TP + projet et des éventuelles absences non justifiées.
> 3. Le [plagiat](https://nuxeo.ipb.fr/nuxeo/nxfile/default/fa82b9dd-f22c-4d41-8ace-5a5e7fa7e60d/blobholder:0/Charte-anti-plagiat.pdf) constitue une fraude dont les conséquences peuvent être graves :
attribution d’une note de zéro au travail incriminé, exclusion de l’établissement, exclusion définitive de tout établissement d’enseignement supérieur français.  
En matière de propriété intellectuelle, le plagiat constitue un délit.
