# Using Augmenta with TouchDesigner

**The goal is to create interactive installations using video mapping and the tracking system Augmenta with TouchDesigner. Augmenta allows you to track people and objects with 3D cameras or LiDAR, with data such as size (as a bounding box), position, velocity and orientation.**

The process will consists of using the basic TouchDesigner file already containing the Augmenta plugin connecting to Augmenta Fusion and using Augmenta data to instance 2D or 3D elements on TouchDesigner.

# 2D instancing

## project1

In the main project, there is the Augmenta base, where you set the OSC port to the one in Augmenta Fusion or in Augmenta Simulator. The outputs are the data coming from Augmenta, and a null showing the debug view.

![Screenshot of inside Top_with_Augmenta node](./images/screen5.png)

There is also the **Top_with_Augmenta**, outputting the final visual.

## *project1 >* Top_with_Augmenta

![Screenshot of inside Top_with_Augmenta node](./images/screen3.png)

In the Composite tab of the **Comp1**, the TOP selected is `Element_replicator/item*/output1` in order to get every output in every items inside the **Element_replicator**.

**Blur1** and **Level4** are used to create the metaballs effect like in [Benjamin Carrier's tutorial](https://www.youtube.com/watch?v=_8DY7myCNgk), with a Pre-Shrink between 1 and 5, and a Filter Size between 10 and 100 in the Blur, and a Brigthness between 1 and 3, and a Contrast betweenn 10 and 80 in the Level.

**Feedback1** and **Level3** are used to create a motion blur, with `Comp3` as the Target TOP of the Feedback, and the Brightness set between 0 and 1 in the Level to change the lenght of the blur.


## *project1 > Top_with_Augmenta >* Element Replicator

![Screenshot of inside Element_replicator node](./images/screen2.png)
In this case, there is 3 distinct objects detected by Augmenta.

If you open any of these items, they contain the same nodes that there are in `Master_Attractor0`, but with the data of one id each.


## *project1 > Top_with_Augmenta > Element Replicator >* Master Attractor

This is the node where the element that will be instanced for each object detected will be created, and Augmenta's datas will be assigned to TOPs parameters.

![Screenshot of inside Master_Attractor0 node](./images/screen1.png)

In this example, we select the position in x and y to assigned it to the circle, as well as the height to set the radius of the circle :

![Screenshot of inside Master_Attractor0 node](./images/screen4.png)

In the Output tab of the TOP (here : `circle1`), the toggle "Comp with Input" needs to be switched on, and in the Common tab, the resolution needs to be the same as the video projector used.