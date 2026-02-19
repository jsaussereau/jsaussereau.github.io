---
title: "07. LCD Etape #2"
date: "2026-02-11T00:00:00+02:00"
summary: "Développemment de la bibliothèque LCD - étape 2 : développement d'une fonction d'envoi de n'importe quelle commande."
menu:
  sidebar:
    name: "LCD Etape #2"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-lcd_2"
    weight: 7
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-lcd_etape2.png"
mermaid: true
draft: false
hidden: false
---


### <ins>Étape 2</ins> : Développement d'une fonction d'envoi de n'importe quelle commande

Maintenant que les accès aux différents champs associés au module LCD sont plus simples à utiliser, la prochaine étape est de développer des fonctions qui pourront être utilisées par toutes les fonctions qui ont besoin d'envoyer une commande au module.

La datasheet du module LCD *DS_Afficheurs_Sunplus* nous apprend que l'on peut câbler ce module LCD à un microcontrôleur sur 8 bits (DB7-DB0) ou 4 bits (DB7-DB4)

> [!TIP]
> Le schéma à la page 19 de la datasheet de la carte PICDEM2+ *DS_PICDEM_2_Plus_Users_Guide* montre comment le module LCD est relié au microcontrôleur. On voit que le bus de données est ici utilisé sur 4 bits.

> [!NOTE]
> Cela n'a pas d'impact sur la taille des commandes que l'on peut envoyer. En effet, même lorsqu'il est cablé sur 4 bits, l'afficheur LCD peut recevoir des commandes avec des données de 4 bits ou bien 8 bits. C'est le protocole de communication qui change.

On va donc développer des fonctions pour ces deux cas :
- `lcd_write_instr_4bits` : envoi d'une commande avec 4 bits de données (pour les premières commandes de l'initialisation)
- `lcd_write_instr_8bits` : envoi d'une commande avec 8 bits de données (pour toutes les commandes classiques)

> [!IMPORTANT]
> Dans les deux cas, pour envoyer ces données, il faut veiller à respecter les chronogrammes à la page 24 de la datasheet *DS_Afficheurs_Sunplus*. 

#### ➤ Commandes 4 bits :
Dans la datasheet du module LCD *DS_Afficheurs_Sunplus* on voit que pour la partie d'initialisation, il y a des commandes avec une données de 4 bits et 2 bits de contrôle.

C'est le cas le plus simple : on écrit 4 bits de données sur un bus de 4 bits.

```c
void lcd_write_instr_4bits(uint8_t rs, uint8_t rw, uint8_t data_4bits) {
    // on assigne les signaux de contrôle
    // on écrit les 4 bits de la donnée
}
```

> [!TIP]
> Pour faire des temporisations on peut utiliser les macros dans la bibliothèque `pic.h` (déjà importée par `xc.h`) :
> - `__delay_us(unsigned int t)` : délai en microsecondes
> - `__delay_ms(unsigned int t)` : délai en millisecondes

`__delay_us` et `__delay_ms` sont implémentées sous forme de boucles qui utilisent le paramètre `_XTAL_FREQ` (fréquence de l'oscilateur du microcontrôleur) pour faire des délais. Il faudra donc penser à le définir :
```c
#define _XTAL_FREQ XXXXXXX // remplacer XXXXXXX par la fréquence du microcontroleur
```

> [!NOTE]
> Il faudra aussi se poser la question de la pertinence des délais les plus courts, au vu de la période l'horloge du microcontrôleur et sachant qu'une instruction assembleur s'exécute en 4 cycles d'horloge...
> Combien de temps s'écoule-t-il entre la fin de l'exécution d'une instruction et la fin de l'exécution de la suivante ?

#### ➤ Commandes 8 bits :

Dans le tableau récapitulatif des commandes du module LCD dans la datasheet *DS_Afficheurs_Sunplus*, on voit que les autres commandes sont composées d'une donnée sur 8 bits et, comme en mode 4 bits, 2 bits de contrôle.

On repart donc sur la même base qu'en mode 4 bits.
La différence est qu'il faut être capable d'envoyer 8 bits de données sur un bus de 4 bits. 

> [!TIP]
> Pour envoyer 8 bits de données sur un bus de 4 bits, pas le choix, il faut envoyer les données en 2 fois.
> Il faut donc faire des [opérations binaires](https://dept-info.labri.fr/ENSEIGNEMENT/programmation1/cours/CM_9___Manipulation_binaire.pdf) afin de séparer les 4 bits de poids fort des 4 bits de poids faible.

> [!IMPORTANT]
> Le protocole exige d'envoyer les bits de poids fort en premier.

```c
void lcd_write_instr_8bits(uint8_t rs, uint8_t rw, uint8_t data_8bits) {
    // on assigne les signaux de contrôle
    // on écrit les 4 bits de poids fort de la donnée
    // on écrit les 4 bits de poids faible de la donnée
}
```

#### ➤ lcd_busy :
Avant d'envoyer une commande il faut s'assurer que le module n'est pas occupé à exécuter la commande précédente. Sinon la commande envoyée ne sera pas exécutée. 

> [!TIP]
> Pour savoir si la précédente commande a fini d'être exécutée, il est possible de lire le bit 'busy', avec la commande correspondante.
> Cependant, dans un premier temps, par souci de simplicité, on peut se contenter d'une temporisation. Sa durée doit à être supérieure au temps d'exécution de la commande la plus longue (cf. *DS_Afficheurs_Sunplus*).

