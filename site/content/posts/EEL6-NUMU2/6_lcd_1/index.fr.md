---
title: "06. LCD Etape #1"
date: "2026-02-11T00:00:00+02:00"
summary: "Développemment de la bibliothèque LCD - étape 1 : simplification des accès."
menu:
  sidebar:
    name: "LCD Etape #1"
    parent: "EEL6-NUMU2"
    identifier: "EEL6-NUMU2-lcd_1"
    weight: 6
author:
  name: "Jonathan Saussereau"
#  image: "/images/"
hero: "EEL6-NUMU2-large-lcd_etape1.png"
mermaid: true
draft: false
hidden: false
---

L'objectif est désormais de développer une bibliothèque pour l'afficheur LCD. Pour cela nous allons procéder par étape, en ajoutant des niveaux d'abstraction au fur et à mesure.
<!-- <h2 id="aide_lib_lcd"> 3. Développement de la bibliothèque pour l'afficheur LCD </h2> -->

### Documentation
Pour développer la bibliothèque LCD, 2 documents seront utiles :
- *DS_PICDEM_2_Plus_Users_Guide* : Datasheet de la carte de développement. Le schéma électrique page 19 permet de comprendre les interconnexions entre le microcontrôleur et le module LCD.
- *DS_Afficheurs_Sunplus* : Datasheet du module LCD. On y trouve la description des entrées/sorties du module (page 4), ansi que les chronogrammes à respecter pour ces signaux (pages 23-24). Toutes les commandes proposées par le module y sont détaillées (pages 5-7). Enfin, la datasheet explicite la procédure d'initialisation du module (pages 10-11). 

### <ins>Étape 1</ins> : Simplification des accès

Pour envoyer des instructions, il faut être capable d'accéder individuellement aux différents champs du port D qui contrôlent l'écran LCD. Il est notamment nécessaire d'écrire sur les 4 bits de données sans modifier les autres bits du port.

> [!TIP]
> Avant toute chose, il est donc vivement recommandé de simplifier les lectures/écritures sur les différents champs du module LCD.  
> Deux approches sont présentées ici.

#### Méthode 1 : Masquage
<details>
<summary>Cliquer ici pour étendre</summary>

Le plus simple est de définir à l'aide de `#define` les alias des différents bits, pour un accès plus clair.  
Les accès au bits se font ainsi simplement par leurs noms. Pour écrire sur plusieurs bits, il faut effectuer un [masquage](https://dept-info.labri.fr/ENSEIGNEMENT/programmation1/cours/CM_9___Manipulation_binaire.pdf).

***Note*** : On a ici la chance de pouvoir accéder aux bits individuellement de base. Sinon, il aurait fallu utiliser le masquage dans tous les cas.

***Exemple*** :

`lib_LCD.h`
```c
#define LCD PORTD
#define E RD6

#define E_ENABLED 0b1
#define E_DISABLED 0b0
```

`lib_LCD.c`
```c
void lcd_write_instr_8bits(uint8_t rs, uint8_t rw, uint8_t data) {
    // [...]
    RS = rs;
    RW = rw;
    
    LCD &= 0xf0; // mise à 0 des 4 bits de poids faible  
    LCD |= (data_MSB & 0x0f); // application des 4 MSB de la donnée sur les 4 LSB de LCD 
    
    E = E_ENABLED;
    // [...]
}
```
</details>

#### Méthode 2 : Champs de bits (bitfields)
Une autre méthode, plus pratique à utiliser, se base sur le même principe d'[union](https://www.tutorialspoint.com/cprogramming/c_unions.htm) de [bitfields](https://www.tutorialspoint.com/cprogramming/c_bit_fields.htm) que dans le fichier d'include `pic16f877a.h`. Il suffit de créer un nouveau type sur le modèle de celui associé au PORTD.

Comme dans le fichier d'include, on peut donner plusieurs définitions aux [bitfields](https://www.tutorialspoint.com/cprogramming/c_bit_fields.htm) grâce aux [unions](https://www.tutorialspoint.com/cprogramming/c_unions.htm). On peut par exemple nommer les bits un à un, et, plus intéressant, définir des champs. Cela rend les lectures/écritures très simples.  

> [!NOTE]
> L'ordre des bits dans les champs de bits n'est hélas pas normé, il dépend du compilateur et de l'architecture. Sur PIC, avec le compilateur XC8, le bit de poids faible (LSB) est en premier et le bit de poids fort (MSB) est en dernier.

***Exemple*** :

`lib_LCD.h`
```c
typedef union {
	// définitions des bits individuellement :
	struct { 
		unsigned DB4             :1;  // bit 0
		unsigned DB5             :1;  // bit 1
		unsigned DB6             :1;  // bit 2
		unsigned DB7             :1;  // bit 3
		unsigned RS              :1;  // bit 4
		unsigned RW              :1;  // bit 5
		unsigned E               :1;  // bit 6
		unsigned POWER           :1;  // bit 7
	}; 
    
	// définition des champs :
	struct {
		unsigned DB              :4;  // bits 0 à 3 : données
		unsigned OPERATION       :2;  // bits 4 et 5 : RS et RW : définissent le type d'opération
		unsigned                 :2;  // E et POWER déjà définis, facultatif
	};
    
} LCDbits_t;

extern volatile LCDbits_t LCDbits @ 0x008; // Définition de "LCDbits" de type LCDbits_t, avec l'adresse du PORTD
```

Grâce au mot-clé [typedef](https://www.tutorialspoint.com/cprogramming/c_typedef.htm), on a ici défini notre propre type `LCDbits_t`, qui est une [union](https://www.tutorialspoint.com/cprogramming/c_unions.htm) de [bitfields](https://www.tutorialspoint.com/cprogramming/c_bit_fields.htm), représentant les champs de l'écran LCD, connectés au port D. 

Pour utiliser ce type sur le port D, on déclare donc une variable `LCDbits` de type `LCDbits_t` à l'adresse `0x008` qui est l'adresse du port D. C'est très exactement de la même manière que `PORTDbits` est défini dans `pic16f877a.h`.

Grâce aux deux définitions spécifiées du port, on a alors plusieurs façons d'accéder à nos signaux :
- On peut accéder au bits individuellement, comme DB5, RS, RW pu E par exemple. 
- On peut accéder à des champs entiers comme DB (DB7-DB4) ou OPERATION (RS,RW)

Exemple d'utilisation :
`lib_LCD.c`
```c
void lcd_write_instr_8bits(uint8_t rs, uint8_t rw, uint8_t data_8bits) {
    // [...]
    LCDbits.RS = rs; 
    // [...]
    LCDbits.DB = data_MSB;
    LCDbits.E = E_ENABLED;
    // [...]
}
```
