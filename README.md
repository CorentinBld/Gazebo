# Gazebo Projet Programmation Robotique (Gazebo-ROS)
## Introduction

Dans le cadre du cours de programmation robotique, on a été amener à réaliser un projet sous Gazebo-ROS. L’objectif était de simuler le comportement d’un robot dans un environnement de notre choix, dans lequel le robot devrait interagir avec l’environnement (bouger, naviguer et éviter les obstacles). Ceci en 3D en utilisant le langage XML et python avec ROS melodic et GAZEBO & RVIZ. 
Dans ce Readme nous allons vous exposer la mise en œuvre du projet, puis ses fonctionnalités ainsi que les difficultés rencontrées lors de la réalisation de celui-ci.
## Mise en œuvre du Projet 
### **Création du Projet**
Dans ce projet nous avons respecter un arbre de création bien précise en fonction de chaque partie du projet.

Image de l'arbre de création 
![](https://i.imgur.com/E6gvC16.png)
 
#### *Organisation du Projet*
Dans l'organisation de ce projet nous avons commencer par créé un dossier cours_ws dans le quelle nous avons créé quatres sous dossier my_robot_description; my_robot_gazebo; my_robot_navigation et my_robot_control. chaqu'un d'eux contiendra des répertoire avec les différent code composent le projet. image de l'arboressence juste audessus.

Pour commencer nous avons créé un modéle de robot.

### **Création du Robot**
#### *Méthode URDF*

Pour créer un robot sur ROS-Gazebo, il est imperatif de faire d'abbord sa description, et pour ce faire on fait récours au fichier URDF (Universal Robot Description File).
Voici un schéma cinématique de notre robot en vue de dessus.
![](https://i.imgur.com/HZOCtJ9.png)

Pour créer le corps du robot, on a; 
    
-créer un package "my robot description" dans          lequel on a mis notre fichier 01-                      description.urdf, qui défini le robot (son nom,        le corps du robot,l'afficheur deu robot, etc.)

-ensuite on crée un fichier "display.launch" dans      lequel on precise le fichier URDF à charger pour      l'afficher, on y met aussi des sliders permetant      de faire bouger les joints du robot.Puis on execute la ligne de commande
```
roslaunch my_robot_description display.launch
```
ce qui affiche une fenetre vide; 
![](https://i.imgur.com/e5R3G1s.png)

Dans la fênetre on ajoute base link et ça affiche la forme de notre robot. 

![](https://i.imgur.com/NQlE5HR.png)

#### *Méthode XACRO*

Une fois qu'on a un modéle URDF, le robot commence à avoir des élémets de plus en plus complexes, on ne peut donc plus coder manuellement, pour cela on fait récours au macros XACRO. Un des avantages du XACRO ets qu'il nous permet de déclarer des variables qui nous facilitera la tâche si l'on veut changer les dimensions d'un ou olusieurs composants du robot.
Le XACRO lit le URDF, et il est appellé ainsi: 
```
$ x ac r o −−i n o r d e r model . x ac r o > model . u r d f
```
la ligne de commande pour lancer le modèle robot.xacro est la même qu'au dessus juste il faut avoir changer le nom du dossier que nous souhaitons ouvrir dans le display.launch
C'est grâce à cette ligne de commande que l'on choisi le fichier à ouvrir 
```
<arg name="model" default="$(find my_robot_description)/urdf/02-description.xacro"/>
```
Modèle avec xacro:
![](https://i.imgur.com/ZIcNQ4P.png)




### **Création du World**
Pour tout simulation de robot il est préférable de faire les tests dans les conditions les plus proche de la réalite. C'est pour cela que nous avons besoin d'un world.
Pour commencer il y la Méthode simple pour crée ce world 
#### *Méthode Simple*
Notre world peux ce composer d'un soleil est d'un plan "ground_plane"
Code disponible juste dessous:
```
<?xml version="1.0"?>
<sdf version='1.6'>
    <world name='default'>
        <include>
            <uri>model://sun</uri>
        </include>
        <model name='ground_plane'>
            <static>1</static>
            <link name='link'>
                <collision name='collision'>
                    <geometry>
                        <plane>
                            <normal>0 0 1</normal>
                            <size>15 15</size>
                        </plane>
                    </geometry>
                    <surface>
                        <friction>
                            <ode>
                                <mu>100</mu>
                                <mu2>50</mu2>
                            </ode>
                        </friction>
                        <bounce/>
                        <contact>
                            <ode/>
                        </contact>
                    </surface>
                    <max_contacts>10</max_contacts>
                </collision>
                <visual name='visual'>
                    <cast_shadows>0</cast_shadows>
                    <geometry>
                        <plane>
                            <normal>0 0 1</normal>
                            <size>40 40</size>
                        </plane>
                    </geometry>
                    <material>
                        <script>
                            <uri>file://media/materials/scripts/gazebo.material</uri>
                            <name>Gazebo/WoodFloor</name>
                        </script>
                    </material>
                </visual>
                <velocity_decay>
                    <linear>0</linear>
                    <angular>0</angular>
                </velocity_decay>
                <self_collide>0</self_collide>
                <kinematic>0</kinematic>
                <gravity>1</gravity>
            </link>
        </model>
    </world>
</sdf>
```
On obtien le résulta sur l'image avec le code du dessus est aprés avoir tapé les commandes suivante.

La premier sert à ajouter le chemin du fichier world 
```
export GAZEBO_RESOURCE_PATH=~/cours_ws/src/my_robot/my_robot_gazebo/world/:$GAZEBO_RESOURCE_PATH
```
La seconde serre a ouvrir le World dans Gazebo
```
roslaunch my_robot_gazebo world.launch
```
Image du monde créé:

![](https://i.imgur.com/Dx4enaB.png)

Avec le world de base que nous venons de crée je peux en faire ce que je veux comme rajouté des obstacles genéré aléatoirement. 
#### *Ajout D'Obstacles*
Pour ajouter des obstacles nous avons choisi la méthode ou nous avons créé un programme qui génére le nombre d'obstacle que nous voulons.
Code disponible en cliquant sur le lien ci-dessous.

[Génerateur d'Obstacle](https://colab.research.google.com/drive/1tZZCMmAjYU4CRFgQJMlaLibpEjnBEvin)

Une fois les obstacles géneré on les ajoute a notre world.
Pour voir le résulta on utilise les commande donnée juste aux dessus on obtient ceux-ci

![](https://i.imgur.com/Xh9OHLd.png)

Nous avons pour finir ajouter des objets maillé à notre world

#### *Ajout D'Objets maillé*
Pour avoir un environement satisfaisant, on a ajouter au géométries canoniques des maillages surfaciques texturés.Pour cela nous avons utilisé la même méthode que pour les objets canoniques vue au dessus 

[Générateur d'objets maillé](https://colab.research.google.com/drive/19Q-QYqiCwub351cEKuQuujDepv4k78kM?usp=sharing)

Résultat obtenue aprés avoir ajouter les objets maillé a notre monde 

![](https://i.imgur.com/cPGNuuT.png)

![](https://i.imgur.com/AY2Ero7.png)

### *Création de l'environnement de simulation*

Pour avoir un environnement de simulation sous ROS dans GAZEBO, on a utilisé le format SDF (Standard Data Format). Elle encapsule une description complète du world comprenant le modèle, la scène, le physique et les plugins. 

### *Ajout des plugins* 


### *Lancement de la simulation*
Enfin une fois que nous avons fait tout les étape explique audessus. Nous avons lancer notre environnement de simulation avec les commande suivante.
pour commencer si cela n'a pas etait fais avant il faudra sourcé avec la ligne de commande suivante 
```
source devel/setup.bash
```
Ensuite pour obtenir l'environement avec notre monde il faud lui tranferait le dossier du world dans le registre ou les monde sont enregitrer. Cette taches ce fais avec la ligne de code suivante
```
export GAZEBO_RESOURCE_PATH=~/cours_ws/src/my_robot/my_robot_gazebo/world/:$GAZEBO_RESOURCE_PATH
```
Pour finir on lance notre environement de simulation avec la commande suivante 
```
roslaunch my_robot_gazebo demo_gazebo.launch 
```
Une fois cette commande tapé l'ouverture peux mettre un peux de temps une fois gazebo ouvert on obtient cela.
image si dessous 

![](https://i.imgur.com/TU72Pi2.png)


## Difficultés rencontrées lors de la réalisasion


