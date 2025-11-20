# TPs - Circuits Numériques et Microcontrôleurs
## Polytech'Paris Sud PeiP2

**Intervenants :**  
- Michèle Gouiffès - michele.gouiffes@universite-paris-saclay.fr  
- Hussein Chaaban - hussein.chaaban@universite-paris-saclay.fr  

> Retrouvez cet énoncé et quelques codes fournis sur [ecampus](https://ecampus.paris-saclay.fr/).

## Introduction
L'objectif de ces TPs est de découvrir un système embarqué et ses fonctionnalités. Nous développerons différentes applications embarquées qui utiliseront les entrées/sorties disponibles de la carte.

## 1. Matériel
La carte NXP LPC1768, basée sur le micro-contrôleur ARM Cortex M3, est utilisée pour le prototypage rapide d'applications embarquées.

### Caractéristiques techniques :
- **Processeur** : ARM Cortex M3 à 96 MHz
- **Mémoire** : 32 Ko RAM, 512 Ko Flash
- **Interfaces** : USB, Ethernet, CAN
- **Périphériques intégrés** :
  - Écran LCD
  - Joystick
  - 2 Potentiomètres
  - Accéléromètre
  - LED RGB
  - Haut-parleur
  - Thermomètre

### Carte de développement
![Carte mbed](figure1.png)

1. Écran LCD
2. Joystick
3. Potentiomètres
4. Accéléromètre
5. LED couleur
6. Thermomètre
7. Communication ZigBee

## 2. Logiciel
### Environnement de développement
1. Créer un compte sur [mbed](https://os.mbed.com/)
2. Utiliser l'[IDE en ligne](https://studio.keil.arm.com/)
3. Créer un nouveau projet (un par exercice)
4. Sélectionner la plateforme : mbed LPC1768
5. Compiler et téléverser le programme

## Exercice 1 : Affichage d'un entier sur les LEDs

### Objectif
Afficher en binaire les nombres de 0 à 15 sur les 4 LEDs de la carte.

### Consignes
1. Déclarer les 4 LEDs comme des objets `DigitalOut`
2. Créer un tableau associé aux 4 LEDs
3. Implémenter les fonctions :
   ```cpp
   void ResetLeds();  // Éteint toutes les LEDs
   void DisplayIntLeds(int i);  // Affiche un nombre de 0 à 15
   ```
4. Programme principal :
   - Afficher en boucle les nombres de 0 à 15
   - Tester avec et sans la fonction `wait(float t)`

## Exercice 2 : Lecture des capteurs/actionneurs

### Objectif
Lire les valeurs des potentiomètres et du thermomètre, et les afficher sur l'écran LCD.

### Consignes
1. Importer les bibliothèques nécessaires :
   ```cpp
   #include "LM75B.h"    // Pour le thermomètre
   #include "C12832.h"   // Pour l'écran LCD
   ```

2. Déclarer les objets :
   ```cpp
   AnalogIn pot1(p19);    // Premier potentiomètre
   AnalogIn pot2(p20);    // Deuxième potentiomètre
   LM75B temp_sensor;     // Capteur de température
   C12832 lcd(p5, p7, p6, p8, p11);  // Écran LCD
   ```

3. Afficher les valeurs sur l'écran :
   - Valeur du potentiomètre 1
   - Valeur du potentiomètre 2
   - Température

4. Visualisation sur LEDs :
   - Convertir la valeur du potentiomètre 1 (0-1) en nombre de LEDs allumées (0-4)
   - Implémenter la fonction :
     ```cpp
     int AnalogIn2Index(float v, int n);
     ```

5. Contrôle de la LED RGB :
   - Associer le potentiomètre 2 aux couleurs
   - Implémenter les fonctions :
     ```cpp
     void R(float v);  // Contrôle la composante rouge
     void G(float v);  // Contrôle la composante verte
     void B(float v);  // Contrôle la composante bleue
     ```

## Exercice 3 : Piano numérique

### Objectif
Créer un piano contrôlé par le clavier du PC.

### Consignes
1. Configuration du haut-parleur :
   ```cpp
   PwmOut speaker(p26);
   ```

2. Lecture des touches du clavier via communication série

3. Contrôle du volume avec le potentiomètre 1

4. Affichage des informations sur l'écran LCD

## Exercice 4 : Chenillard contrôlé par joystick

### Objectif
Créer une animation de chenillard contrôlable avec le joystick.

### Commandes
- **→ (R)** : Décalage à droite
- **← (L)** : Décalage à gauche
- **↑ (U)** : Allumer une LED
- **↓ (D)** : Éteindre une LED
- **• (X)** : Pause/Arrêt

### 4.1 Décalage à droite

#### Fonction de décalage
```cpp
void ShiftRight() {
    // Décalage d'un bit vers la droite
    // Le LSB disparaît à droite et réapparaît à gauche (MSB)
    val = (val >> 1) | ((val & 0x01) << 3);
}
```

#### Allumage d'une LED
```cpp
void LedOnRight() {
    // Allume une LED à la fin du chenillard
    val |= (val & 0x08) >> 3;
}
```

#### Extinction d'une LED
```cpp
void LedOffRight() {
    // Éteint la dernière LED allumée
    val &= ~(0x01 << (3 - (val & 0x03)));
}
```

### 4.2 Gestion des commandes du joystick

#### Configuration des entrées
```cpp
DigitalIn left(p13);    // Bouton gauche
DigitalIn right(p16);   // Bouton droit
DigitalIn up(p15);      // Bouton haut
DigitalIn down(p12);    // Bouton bas
DigitalIn center(p14);  // Bouton central (X)
```

#### Programme principal
```cpp
int main() {
    // Initialisation
    val = 0x01;  // Première LED allumée
    
    while(1) {
        // Lecture des boutons
        if (left) {
            // Mode décalage à gauche
            direction = LEFT;
        } 
        else if (right) {
            // Mode décalage à droite
            direction = RIGHT;
        }
        
        // Gestion des commandes selon la direction
        if (direction == RIGHT) {
            if (up) LedOnRight();
            if (down) LedOffRight();
            ShiftRight();
        } 
        else {
            if (up) LedOnLeft();
            if (down) LedOffLeft();
            ShiftLeft();
        }
        
        // Mise à jour des LEDs
        DisplayIntLeds(val);
        
        // Gestion de la pause
        if (center) {
            PauseStop();
        }
        
        // Temporisation
        wait(0.2);
    }
}
```

### 4.3 Fonction de pause

#### Implémentation
```cpp
void PauseStop() {
    Timer t;
    t.start();
    
    // Tant que le bouton est pressé
    while (center) {
        // Si appui > 3 secondes
        if (t.read() > 3.0) {
            // Éteint toutes les LEDs
            val = 0x00;
            DisplayIntLeds(val);
            return;
        }
        wait(0.1);
    }
    
    // Si simple pression < 3s
    if (t.read() < 3.0) {
        // Mise en pause
        bool paused = true;
        while (paused) {
            if (center) {
                // Sortie de la pause
                while(center);  // Attente relâchement
                paused = false;
            }
            wait(0.1);
        }
    }
    
    t.stop();
}
```

### 4.4 Améliorations possibles

1. **Gestion du débordement** :
   ```cpp
   // Pour éviter les valeurs négatives
   val = val & 0x0F;
   ```

2. **Anti-rebond** :
   ```cpp
   bool buttonPressed(DigitalIn &button) {
       if (button) {
           wait(0.05);  // Délai d'anti-rebond
           return button;
       }
       return false;
   }
   ```

3. **Vitesse variable** :
   ```cpp
   float speed = 0.2;  // Vitesse par défaut
   // ...
   wait(speed);  // Utilisation dans la boucle principale
   ```

## Exercice 5 : Jeu de billes

### Objectif
Créer un jeu contrôlé par l'inclinaison de la carte.

### Fonctionnalités
- Détection de l'inclinaison avec l'accéléromètre
- Déplacement de la bille en fonction de l'inclinaison
- Système de score et de niveaux

## Projets optionnels

### 6.1 Snake
- Contrôle au joystick
- Logique de jeu
- Gestion de la difficulté

### 6.2 Tétris
- Affichage sur LCD
- Gestion des pièces
- Système de score

## Ressources
- [Documentation Mbed OS](https://os.mbed.com/docs/)
- [Bibliothèque LM75B](https://os.mbed.com/users/chris/code/LM75B/)
- [Bibliothèque C12832](https://os.mbed.com/users/chris/code/C12832/)
- [Documentation MMA7660](https://os.mbed.com/components/MMA7660/)
