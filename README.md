![](./resources/official_armmbed_example_badge.png)
# Snake Game - Accéléromètre Mbed OS

Ce projet implémente un jeu Snake contrôlé par l'accéléromètre sur la carte NXP LPC1768. Le serpent se déplace en fonction de l'inclinaison de la carte, détectée par l'accéléromètre intégré. Le jeu utilise l'écran LCD pour l'affichage et les LEDs pour les indicateurs de score.

You can build the project with all supported [Mbed OS build tools](https://os.mbed.com/docs/mbed-os/latest/tools/index.html). However, this example project specifically refers to the command-line interface tools, [Arm Mbed CLI 1](https://github.com/ARMmbed/mbed-cli#installing-mbed-cli) and [Mbed CLI 2](https://github.com/ARMmbed/mbed-tools#installation).

(Note: To see a rendered example you can import into the Arm Online Compiler, please see our [import quick start](https://os.mbed.com/docs/mbed-os/latest/quick-start/online-with-the-online-compiler.html#importing-the-code).)

## Mbed OS build tools

### Mbed CLI 2
Starting with version 6.5, Mbed OS uses Mbed CLI 2. It uses Ninja as a build system, and CMake to generate the build environment and manage the build process in a compiler-independent manner. If you are working with Mbed OS version prior to 6.5 then check the section [Mbed CLI 1](#mbed-cli-1).
1. [Install Mbed CLI 2](https://os.mbed.com/docs/mbed-os/latest/build-tools/install-or-upgrade.html).
1. From the command-line, import the example: `mbed-tools import mbed-os-example-blinky`
1. Change the current directory to where the project was imported.

### Mbed CLI 1
1. [Install Mbed CLI 1](https://os.mbed.com/docs/mbed-os/latest/quick-start/offline-with-mbed-cli.html).
1. From the command-line, import the example: `mbed import mbed-os-example-blinky`
1. Change the current directory to where the project was imported.

## Fonctionnalités du jeu

Le jeu Snake utilise les composants suivants de la carte LPC1768 :
- **Accéléromètre MMA7660** : Contrôle la direction du serpent par inclinaison de la carte
- **Écran LCD C12832** : Affichage du jeu, score et informations
- **LEDs** : Indicateurs visuels du score et état du jeu
- **Boutons** : Contrôles supplémentaires (pause, redémarrage)

### Contrôles
- **Inclinaison de la carte** : Change la direction du serpent (haut, bas, gauche, droite)
- **Bouton central (joystick)** : Pause/reprise du jeu
- **Bouton haut** : Redémarrage du jeu

### Règles du jeu
- Le serpent grandit en mangeant la nourriture (représentée par un carré)
- Éviter les collisions avec les murs ou le corps du serpent
- Le score augmente à chaque nourriture mangée
- Vitesse progressive pour augmenter la difficulté

**Note**: Ce projet nécessite un target avec support RTOS.

## Building and running

1. Connect a USB cable between the USB port on the board and the host computer.
1. Run the following command to build the example project and program the microcontroller flash memory:

    * Mbed CLI 2

    ```bash
    $ mbed-tools compile -m <TARGET> -t <TOOLCHAIN> --flash
    ```

    * Mbed CLI 1

    ```bash
    $ mbed compile -m <TARGET> -t <TOOLCHAIN> --flash
    ```

Your PC may take a few minutes to compile your code.

The binary is located at:
* **Mbed CLI 2** - `./cmake_build/<TARGET>/develop/<TOOLCHAIN>/mbed-os-example-blinky.bin`
* **Mbed CLI 1** - `./BUILD/<TARGET>/<TOOLCHAIN>/mbed-os-example-blinky.bin`

Alternatively, you can manually copy the binary to the board, which you mount on the host computer over USB.

## Fonctionnement attendu
- Le serpent apparaît au centre de l'écran LCD
- La nourriture est représentée par un carré qui apparaît aléatoirement
- Inclinez la carte pour déplacer le serpent dans la direction souhaitée
- Le score s'affiche en temps réel sur l'écran
- Les LEDs indiquent le niveau de score (plus de LEDs = meilleur score)
- Game Over s'affiche lorsque le serpent touche un mur ou lui-même

## Composants utilisés
- **MMA7660** : Accéléromètre pour le contrôle du serpent
- **C12832** : Écran LCD pour l'affichage du jeu
- **DigitalOut** : LEDs pour les indicateurs visuels
- **DigitalIn** : Boutons du joystick pour les contrôles
- **Timer** : Gestion du timing du jeu

## Troubleshooting
If you have problems, you can review the [documentation](https://os.mbed.com/docs/latest/tutorials/debugging.html) for suggestions on what could be wrong and how to fix it.

## Liens utiles

* [Documentation Mbed OS](https://os.mbed.com/docs/).
* [Bibliothèque MMA7660 (Accéléromètre)](https://os.mbed.com/components/MMA7660/).
* [Bibliothèque C12832 (Écran LCD)](https://os.mbed.com/users/chris/code/C12832/).
* [Guide de programmation Mbed OS](https://os.mbed.com/docs/mbed-os/latest/apis/index.html).
* [Carte NXP LPC1768](https://os.mbed.com/platforms/mbed-LPC1768/).

### License and contributions

The software is provided under Apache-2.0 license. Contributions to this project are accepted under the same license. Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for more info.

This project contains code from other projects. The original license text is included in those source files. They must comply with our license guide.
