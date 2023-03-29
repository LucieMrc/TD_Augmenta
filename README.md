# Using Augmenta with TouchDesigner

**The goal is to create interactive installations using video mapping and the tracking system Augmenta with TouchDesigner. Augmenta allows you to track people and objects with 3D cameras or LiDAR, with data such as size (as a bounding box), position, velocity and orientation.**

The process will consists of using the basic TouchDesigner file already containing the Augmenta plugin connecting to Augmenta Fusion and using Augmenta data to instance 2D or 3D elements on TouchDesigner.

## Content
* [Augmenta Simulator](#Augmenta-Simulator)<br>

* [Augmenta Fusion : Set up and calibration](#Augmenta-Fusion--Set-up-and-calibration)<br> 
    * [Set up of the scene and projection](#Set-up-of-the-scene-and-projection)<br>
    * [Web Interface](#Web-Interface)<br>
        * [3D Camera](#3D-camera)<br>
        * [Lidar](#lidar)<br>
    * [Calibration](#Calibration)<br>

* [2D instancing in TD](#2D-instancing-in-TD)    
    * [project1](#project1)<br>
    * [Top_with_Augmenta](#project1--top_with_augmenta)<br>
    * [Element Replicator](#project1--top_with_augmenta--element-replicator)<br>
    * [Master Attractor](#project1--top_with_augmenta--element-replicator--master-attractor)<br>



# Augmenta Simulator

To start creating from Augmenta datas without the set up (from home, before setting up, etc), you can use the software Augmenta Simulator. It simulate people moving on a scene, with their tracking datas being sent via OSC to whatever creative software you want to use.

[Download Augmenta Simulator](https://augmenta.tech/downloads/)

![Schema](./images/screen10.png)

You simply click on the scene to create a person, and right-click on it to delete it.

On the settings on the left pannel, you can set up the width and height of the scene in meters, based on the size of your scene in real life.
You can also decide the speed of the people moving, and their minimal and maximal size.

# Augmenta Fusion : Set up and calibration

## Set up of the scene and projection

Open AugmentaFusion.

You need to know the size in meters of your scene (the size of the projection on the ground/wall), and set it up in the Inspector of the scene.

![Schema](./images/screen6.png)

You can then output your Augmenta scene via Syphon or Spout on the projector (with the same resolution as your projector), to make sure the set up is correct.

![Schema](./images/screen7.png)

If the set up is correct, the squares projected are actually squares.

If your connection works, you should see one source available in the `Sources` tab. Add it to see the source surface appear on the scene.

Right-click on the source to `Open Web Interface`.

## Web Interface

### 3D camera
XYZ position of points, calculated into bounding boxes around objects :

![Schema](./images/schema2.png)

On the web interface, you can see the depth that the camera sees in greyscale, and adjust the parameters.

![Schema](./images/synoptique2.png)

### Lidar
XY position of points :

![Schema](./images/schema.png)

Your network should be lidar > RJ45 > lan2 of the node > lan 1 of the node > RJ45 > lan 2  of the router > lan 1  of the router > computer.

<!-- (jsuis pas sure de l'ordre sur le routeur). -->

![Schema](./images/synoptique1.png)

To have the right IP adress, you need to change your ethernet network parameters when you are connected to the router.
Change your IP adress from DHCP to manual, and choose 192.168.8.3(or anything from 192.168.8.3 to 192.168.8.254), subnet mask 255.255.0.0, and the router is at 192.168.8.1.
![Schema](./images/screen9.png)

The source should be detected on AugmentaFusion, and you can then open the web interface. It will detect that you are using a Lidar, and you can open the Lidar interface.

On the web interface, you can connect to the lidar with the IP 192.168.0.15, on the port 10940, check out the output of what the lidar sees, and determine the size of the detection area in meters.
You can also select the minimum object size to be detected and the noise threshold.

![Schema](./images/screen8.png)

## Calibration

Ask someone to enter the interaction area (touching the wall where the lidar is, stepping under the camera), you should see them appear on the source surface on the scene on AugmentaFusion.

Translate their point by dragging the source surface on the scene until it matches the position on the projection.

While holding alt, drag and drop the + anchor of the source surface to the point.

Ask the someone the move to the other side of the interaction area, and drag to rotate and scale the source surface until the point on the projection is on the new position.

Now when people move on the projection area, points should appear on their position.

<!-- ajouter un gif -->

# 2D instancing in TD

## project1

In the main project, there is the Augmenta base, where you set the OSC port to the one in Augmenta Fusion or in Augmenta Simulator. The outputs are the data coming from Augmenta, and a null showing the debug view.

![Screenshot of inside Top_with_Augmenta node](./images/screen5.png)

There is also the **Top_with_Augmenta**, outputting the final visual.

## *project1 >* Top_with_Augmenta

The `TOP with Augmenta` Base is used to generate and modify elements that are generated for each person detected.

![Screenshot of inside Top_with_Augmenta node](./images/screen3.png)

In the Composite tab of the **Comp1**, the TOP selected is `Element_replicator/item*/output1` in order to get every output in every items inside the **Element_replicator**.

**Blur1** and **Level4** are used to create the metaballs effect like in [Benjamin Carrier's tutorial](https://www.youtube.com/watch?v=_8DY7myCNgk), with a Pre-Shrink between 1 and 5, and a Filter Size between 10 and 100 in the Blur, and a Brigthness between 1 and 3, and a Contrast between 10 and 80 in the Level.

**Feedback1** and **Level3** are used to create a motion blur, with `Comp3` as the Target TOP of the Feedback, and the Brightness set between 0 and 1 in the Level to change the lenght of the blur.

## *project1 > Top_with_Augmenta >* Element Replicator

![Screenshot of inside Element_replicator node](./images/screen2.png)
In this case, there is 3 distinct objects detected by Augmenta.

If you open any of these items, they contain the same nodes that are in `Master_Attractor0`, but with the data of one id each.

The items here are the children of the `Master_Attractor0` and the `Replicator`, where the `Master_Attractor0` is the template and the `Replicator` repeat this template for every row of the array of people detected.

## *project1 > Top_with_Augmenta > Element Replicator >* Master Attractor

This is the node where the element that will be instanced for each object detected will be created, and Augmenta's datas will be assigned to TOPs parameters.

The `Select` DAT get the array of people detected, and the `Select` CHOP allows to select specific column in the array (here the x, y with index between 1 and 2, and the height with the index 11).

![Screenshot of inside Master_Attractor0 node](./images/screen1.png)

In this example, we select the position in x and y to assign it to the circle, as well as the height to set the radius of the circle :

![Screenshot of inside Master_Attractor0 node](./images/screen4.png)

In the Output tab of the TOP (here : `circle1`), the toggle "Comp with Input" needs to be switched on, and in the Common tab, the resolution needs to be the same as the video projector used.