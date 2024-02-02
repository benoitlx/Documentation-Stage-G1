---
date: 2024-02-02
aliases: 
tags: 
draft: true
---

Bonjour,

Je suis actuellement entrain d'interfacer des modules KSG101 et KPZ101 afin de pouvoir les controller en python. Jusqu'ici, j'ai suivi la documentation du protocol APT pour parvenir à mes fins. J'ai pu établir une connexion avec les modules et exécuter différentes fonctions et récupérer les informations donnés par les modules, et la communication à l'air de vraiment bien marcher. J'aurais tout de même besoin de précision pour interpréter des donnés reçues du KSG101 et sur la commande pour atteindre une position avec le KPZ101.

Mon premier questionnement est le suivant: comment passer de l'entier (short) `Reading` obtenue par la commande `MGMSG_PZ_GET_TSG_READING` au déplacement par rapport au zéro (établi avec la commande `MGMSG_PZ_SET_ZERO`). De ce que j'ai compris de la documentation, il suffirait de faire `Reading*MAX_TRAVEL/32767` pour retrouver la position. Or la position donné par ce calcul (avec `MAX_TRAVEL=20micromètre` donné par la commande `MGMSG_PZ_GET_MAXTRAVEL`) ne correspond pas avec ce qui est indiqué par l'écran du KSG101. Autre petite question, à quoi sert l'entier (word) `Smoothed` de la même commande ? 

Concernant le KPZ101 et la commande `MGMSG_PZ_SET_OUTPUTPOS` (le module est bien configuré en boucle fermée avec le KSP101 comme source de feedback), j'ai compris que l'entier (word) `PositionSW` était à valeur dans 0..32767 et que 0 correspondrait au zéro initialisé avec la commande `MGMSG_PZ_SET_ZERO` du KSG101, et que 32767 correspondrait à un déplacement de `MAX_TRAVEL=20micromètre` par rapport au zéro. Mon problème et que je n'observe pas ce comportement en observant ce déplacement sous microscope (le plateau se déplace de 10micromètre pour `PositionSW=32767`), la source d'incompréhension la plus vraisemblable est que je n'ai pas bien compris la documentation, et dans ce cas merci de m'éclairer. Autre question : pourquoi l'afficheur du KPZ101 n'indique pas un déplacement de 0% avec `PositionSW=0`. Autre remarque j'ai testé d'envoyer un `PositionSW` à valeur dans -32768..0 et j'arrive à me déplacer dans l'interval [zéro+20micromètre, zéro+10micromètre] (voir photo), pourquoi un tel comportement ?

Je ne m'attends pas à ce que vous répondiez à toutes mes interrogations, mais j'aimerais au moins comprendre les deux principales questions posés.

Merci d'avance pour votre aide,

Benoit Leroux