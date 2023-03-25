# Introduction
Sur Arkania, nous utilisons les querys via des async-task, se qui nous permet une meilleur optimisation
et un fonctionnement global et commun à chaque développeur.

# Qu'est ce qu'une AsyncTask ?

* Une Async-Task ou AsyncroneTask est une task qui va s'exécuter sur un thread à part du thread principale
et donc évité de le ralentir avec des actions consomatrices. A savoir aussi que quand un thread n'est plus utilisé
il se supprime et les informations contenue avec.

# Comment utiliser une AsyncTask ?

* Pour utiliser une async task c'est très simple, il vous suffit de créer un fichier ``.php`` que vous nommez 
comme vous le souhaitez puis de mettre ce code :

````php
<?php

namespace <namespace>

use pocketmine\...\AsyncTask;

class <nom de votre classe> extend AsyncTask {

/* faites un constructeur si besoin */

public function onRun() : void {

// Cette fonction est oblogatoire dans les tasks async et doit être mise en première.
//C'est celle qui sera exécuté dans un thread à part.

/* /!\ A savoir que le thread principale ne peut pas communiquer avec d'autre thread et inversement */

public function onCompletion(): void {

// Cette fonction est exécuté dans le thread principale.
// La seul façons de comminiquer des informations 
// avec le thread principale est d'utiliser la fonction getResult() et setResult().

}

}

}
````

