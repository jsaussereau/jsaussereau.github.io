---
title: "3. Evaluation"
date: "2026-02-11T00:00:00+02:00"
summary: "Modalités d'évaluation et de remise des travaux."
weight: 3
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
hero_dark: "EEL6-NUMU2-large-evaluation-dark.png"
mermaid: true
draft: false
hidden: false
---


## Modalités de rendu

Le rendu du code source et du rapport est à faire [sur Moodle](https://moodle.bordeaux-inp.fr/course/view.php?id=3963), **par un seul des membres du binôme**, avant le **24 mai 23:59**.

> [!WARNING]
> Veillez à ce que les deux noms du binôme apparaissent dans le rapport !

> [!IMPORTANT]
> Merci de bien téléverser vos fichiers dans la rubrique de rendu correspondant à votre groupe de TP !

## Ressources à rendre

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
- Pour l'explication des fonctions, il ne s'agit pas de juste expliquer ce qu'elles font fonctionnellement (ça c'est le cahier des charges, que l'on a déjà). Il s'agit d'expliquer **comment elles ont été implémentées** (opérations utilisées, logique, optimisations, ...) et pourquoi elles sont implémentées comme ça et pas autrement.
- De manière générale, dès que des choix on été fait, comme des choix de configuration de module, ou des choix d'implémentation, expliquer ces choix, **même quand plusieurs configurations correspondaient aux exigences**. 
- La description de `lcd_write_instr_4bits`, `lcd_write_instr_8bits` et `lcd_init` pourra être accompagnée d'organigrammes expliquant le déroulement de ces fonctions.
- La description de la partie de configuration de l'horloge pourra être accompagnée d'un diagramme de la machine d'état.
- Bien faire apparaître, et justifier, ce qui est effectué par interruption et ce qui ne l'est pas.
- Même si vous n'avez pas réalisé une partie, vous pouvez l'indiquer et expliquer comment vous auriez fait.
- Bon courage ;)

> [!CAUTION]  
> 1. Le code source doit compiler !
> 2. La note qui vous sera attribuée sur ce module tient également compte du travail observé durant les séances de TP + projet et des éventuelles absences non justifiées.
> 3. Le [plagiat](https://ent.bordeaux-inp.fr/_plugins/flipbook/intranet/_attachments-flipbook/nouvelle-faq-87/Charte-anti-plagiat.pdf/_contents/ametys-internal%253Asites/intranet/ametys-internal%253Acontents/nouvelle-faq-87/ametys-internal%253Aattachments/Charte-anti-plagiat.pdf/book.html) constitue une fraude dont les conséquences peuvent être graves :
attribution d’une note de zéro au travail incriminé, exclusion de l’établissement, exclusion définitive de tout établissement d’enseignement supérieur français.  
En matière de propriété intellectuelle, le plagiat constitue un délit.
