---
title: "05. Clignotement de la LED"
date: "2026-02-11T00:00:00+02:00"
summary: "Implémentation du clignotemment d'une LED à la fréquence 0.5 Hz à partir d’interruptions sur le Timer1."
menu:
  sidebar:
    name: "Clignotement de la LED"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-led"
    weight: 5
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-led.png"
mermaid: true
draft: false
hidden: false
---

<!-- <h2 id="aide_led"> 2. Clignotement de la LED à la fréquence 0.5 Hz </h2> -->

Le tout premier objectif est de faire clignoter une des LED à la fréquence 0.5 Hz (1 changement d’état toutes les secondes) à partir d’interruptions sur le Timer1. 

### Choix de la stratégie

Nous avons ici une exigence de précision, alors comme en TP, c'est la stratégie par interruption qui s'impose face à l'attente active.

Pour générer les interruptions qui ferront clignoter la LED, nous avons le choix entre les 3 timers du microcontrôleur. 

La datasheet *DS_PIC16F877A* nous apprend qu'il est possible de configurer le Timer 1 sur une horloge externe. 
Le schéma à la page 19 de la datasheet de la carte PICDEM2+ *DS_PICDEM_2_Plus_Users_Guide* montre qu'il y a un quartz (Y3) de 32.768 kHz ($2^{15}$ Hz) relié aux broches OSO (RC0) et OSI (RC1) du PIC. 

> [!TIP]
> L'incrémentation d'un compteur 16 bits comme le Timer 1 à une fréquence de $2^{15}$ Hz permettrait un débordement de celui-ci toutes les 2 secondes ($2^{16} / 2^{15}$) très précisément. 
> Le Timer 1 est donc un bon candidat pour cette application.


### Développement d'une bibliothèque pour le Timer 1

La section "Timer1" de la datasheet du microcontrôleur *DS_PIC16F877A* détaille le fonctionnement de ce timer, avec notamment un schéma de son fonctionnement et un tableau regroupant les registres à utiliser pour le configurer.

On cherche tout d'abord à définir à l'aide de `#define` dans `timer.h` (Header Files) les différentes valeurs des champs associés au Timer 1. Cela permettra d'avoir juste à choisir la bonne constante lors de la configuration du timer.

En plus de faciliter l'étape de configuration, l'objectif est que par la suite chaque ligne de code soit compréhensible sans avoir à regarder la datasheet.

#### Exemple de ce qu'il **ne faut PAS** faire :
`timer.c`
```c
void timer_init() {
	T1CON = 0x4c;
}
```
La notation est compacte mais on ne comprend rien à ce qu'il se passe... 
Sans la datasheet sous les yeux et un effort de compréhension (avec risque d'erreur), impossible de savoir à quelle configuration correspond cette valeur. 

> [!NOTE]
> Dans le milieu professionnel, les codes sont écrits par plusieurs personnes et doivent pouvoir être repris par n'importe qui dans le futur. Il donc est impensable de proposer un tel code en entreprise.

#### Exemple de notation bien plus lisible :
`timer.h`
```c
//T1CONbits.T1CKPS :
#define T1_PRESCALER_DIV8 0b11
#define T1_PRESCALER_DIV4 0b10
#define T1_PRESCALER_DIV2 0b01
#define T1_PRESCALER_DIV1 0b00

//...
```
`timer.c`
```c
void timer_init() {
	T1CONbits.T1CKPS = T1_PRESCALER_DIV8; // Division par 8 car ...
	T1CONbits.T1OSCEN = ...;
}
```

L'exemple ci dessus met en oeuvre plusieurs bonnes pratiques :
- L'initialisation des champs indépendaments les uns des autres plutôt que d'écrire sur tout le registre : le code est plus simple à comprendre et à modifier.
- L'utilisation de constantes avec des noms signifiants : permet de comprendre facilement quel est le rôle de la valeur qui a été mise dans le champs.

> [!NOTE]
> Cette notation demande un effort supplémentaire lors de l'écriture du programme mais rendra le débuggage et la modification tellement plus facile. Ici, même sans la datasheet, on comprend ce qu'il se passe.
> De plus, les commentaires sont un bon moyen de se rappeler pourquoi on a fait tel ou tel choix.

### Configuration du timer

Il ne reste plus qu'à utiliser ces définitions pour l'initialisation du timer dans la fonction `timer_init` (`timer.c`).

L'objectif étant dans un premier temps de le configurer pour qu'il déclenche une interruption toutes les deux secondes.

La section `TIMER1 MODULE` de la datasheet *DS_PIC16F877A* décrit le fonctionnement du Timer1. Le schéma page 58 aide à comprendre son fonctionnement et le rôle de chaque champ de configuration.
> [!TIP]
> Le tableau à la page 60 de la datasheet *DS_PIC16F877A* met en évidence tous les champs liés au Timer1. Pour être sûr de l'avoir bien configuré, il faut être sûr de comprendre quel est le rôle de chacun de ces champs et être sûr d'avoir assigné la bonne valeur aux champs qui en ont besoin.

Une fois le timer configuré, on veut déclencher une interruption à chaque débordement. Il faut aussi configurer notre fonction d'interruption pour faire changer d'état la LED à chaque interruption.
> [!TIP]
> Le schéma page 153 permet de visualiser les conditions à remplir pour qu'une interruption se déclenche. 

> [!NOTE]
> Une fois les interruptions configurées, on peut déjà essayer sur la carte pour voir si on a bien la LED qui change d'état toutes les 2 secondes.

### Configuration du module CCP

Pour avoir un débordement toutes les secondes (et plus toutes les 2 secondes), on peut utiliser un module de comparaison CCP.  
Le but est ici d'utiliser ce module pour générer une interruption à chaque fois que la valeur du compteur du Timer1 est à mi-parcours (entre deux interruptions de débordement). Il faut donc également activer les interruptions sur le module CCP.  

Les modules CCP peuvent être utilisés dans différentes configurations, il faut choisir la plus adaptée au besoin.  
Il faut aussi penser à bien mettre une valeur à comparer.

Comme pour la configuration du timer, la configuration du module CCP se fera a l'aide de constantes définies au préalable.

> [!TIP]
> Le tableau à la page 68 de la datasheet *DS_PIC16F877A* met en évidence tous les champs liés au module CCP. Pour être sûr de l'avoir bien configuré, il faut être sûr de comprendre quel est le rôle de chacun de ces champs et être sûr d'avoir assigné la bonne valeur aux champs qui en ont besoin.

