# Gazebo Projet Programmation Robotique (Gazebo-ROS)
## Introduction

Dans le cadre du cours de programmation robotique, on a été amener à réaliser un projet sous Gazebo-ROS. L’objectif était de simuler le comportement d’un robot dans un environnement de notre choix, dans lequel le robot devrait interagir avec l’environnement (bouger, naviguer et éviter les obstacles). Ceci en 3D en utilisant le langage XML et python avec ROS melodic et GAZEBO & RVIZ. 
Dans ce Readme nous allons vous exposer la mise en œuvre du projet, puis ses fonctionnalités ainsi que les difficultés rencontrées lors de la réalisation de celui-ci.
## Mise en œuvre du Projet 
### **Création du Projet**
#### *Organisation du Projet*

### **Création du Robot**
#### *Méthode URDF*

```
ligne de code
```



#### *Méthode XACRO*

```
ligne de code
```


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

La premier serre pour ajouter le chemin du fichier world 
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

Nous avons pour finir ajouter des objets mallet à notre world

#### *Ajout D'Objets mallet*








## h2













## Difficultés rencontrées lors de la réalisation


