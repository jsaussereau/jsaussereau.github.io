---
title: "12. Configuration de l'horloge"
date: "2026-02-11T00:00:00+02:00"
description: ""
menu:
  sidebar:
    name: "Configuration de l'horloge"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-configuration"
    weight: 12
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-configuration.png"
mermaid: true
draft: false
hidden: false
---

<h2 id="aide_conf_horloge"> 5. Développement de la fonctionnalité de configuration de l'horloge </h2>

### Rappel du cahier des charges

L'objectif de cette partie est de pouvoir configurer l'horloge à l'aide des boutons poussoir S2 et S3 :
- Un appui prolongé d'au moins 2 s sur S2 fera clignoter les heures, celles-ci s'incrémenteront à chaque appui sur S3, et automatiquement (f ≈ 5 Hz) en cas d'appui maintenu au-delà de 2 s.
- Un nouvel appui sur S2 permettra un réglage des minutes selon la même procédure.
- Éventuellement, un nouvel appui sur S2 permettra un réglage des secondes selon la même procédure.
- Un dernier appui sur S2 fera quitter le mode "réglage".

> [!TIP]
> Un bonne approche pour implémenter cette fonctionnalité est de développer une machine d'état.

> [!TIP]
> Les interrupteurs physiques ont une période de rebonds qui peut provoquer des faux déclenchements. Pour éviter des changements d'états non désirés, un filtre anti-rebonds peut être implémenté à l'aide de délais de temporisation.

### Machine d'état en C

Le principe est le même que dans un langage de description matérielle comme le VHDL : représenter le comportement d'un système en réponse à des événements.

Voici un exemple de machine d'état implémentant le système de contrôle d'une porte automatique.

```c
// Déclaration de l'enumération représentant les états
enum state_e {
    ST_CLOSED,
    ST_OPENED,
    ST_CLOSING,
    ST_OPENING
};

enum state_e state = ST_CLOSED; // Déclaration et initialisation de la variable contenant l'état

void main() {
    while (1) {
        switch(state) {
            case ST_CLOSED:
                // Condition de changement d'état
                if (BTN_OPEN == BTN_ON) {
                    state = ST_OPENING;
                }
                break;
            case ST_OPENED:
                // Comportement si le système est dans cet état
                if (BTN_OPEN == BTN_ON) {
                    reset_timer_opened();
                }
                // Condition de changement d'état
                if (timer_opened >= OPENED_DURATION) {
                    state = ST_CLOSING;
                }
                break;
            case ST_CLOSING:
                // Comportement si le système est dans cet état
                move_door(DIRECTION_CLOSE);

                // Condition de changement d'état
                if (SENSOR_SECURITY == PERSON_DETECTED || BTN_OPEN == BTN_ON) {
                    state = ST_OPENING;
                } else if (SENSOR_CLOSED == SENSOR_ON) {
                    state = ST_CLOSED;
                }
                break;
            case ST_OPENING:
                // Comportement si le système est dans cet état
                move_door(DIRECTION_OPEN);

                // Condition de changement d'état
                if (SENSOR_OPENED == SENSOR_ON) {
                    reset_timer_opened();
                    state = ST_OPENED;
                }
                break;
        }
    }
}
```