To generate this outline, start by creating a line layer via *Layer > Create Layer > New Shapefile Layer...*. This will serve as our "scratch" layer for generating our guiding lines - think of it like a sketch before you draw the main polygons.
**Note:** QGIS has a "scratch layer" feature, but these are cleared at the end of a session. We use line-type shapefile layers so that they persist throughout sessions and can be used in other projects.

On the New Shapefile Layer popup window, select the *Line* radio button, ensure that your *CRS* is set to *3857* (unless you're specifically using a different CRS) then hit OK.
Set your file path and line layer name as you see fit - e.g. something like "scratch" or "guide" - and confirm.
Your new line layer should now be present in the *Layers* panel on the left of the window.
Click and drag this layer to the top of the list, to ensure that it will be visible over our other layers.
Using *Right click > Properties > Style* you can change the colour of this layer to make sure it's visible - bright red or green tend to work well.

With your scratch layer set up, you can begin to draw the guide lines for your outline's perimeter.

- Click the “Toggle Editing” button <img src="/images/tutorial/buttons/edit_toggle.png">

- Click the “Add Features” button <img src="/images/tutorial/buttons/line_add_feature.png">

- We also use the *Snapping Tool* and *Advanced Digitizing Panel* to ensure that we can make points accurate, and keep walls parallel and perpendicular to one another, where appropriate.
- Set the snapping parameters via *Settings > Snapping Options...*
- More often that not, our settings are something similar to the following: 

<p align="center">
<img src="/images/tutorial/snapping_panel.png" width=400>
</p>

- These settings ensure that your can snap to any corner or edge on any active layer, including those that you're not currently working on (which will be important later).

- Ensure that Advanced Digitizing is active via *View > Toolbars > Advanced Digitizing Toolbar*
- You may notice that there's a new button available while your layer is in edit mode: the *Enable advanced digitizing tools* button. <img src="/images/tutorial/buttons/digitization_toggle.png">

Your first line is going to be your "base line" - i.e. the source line for keeping your walls and rooms parallel and perpendicular to one another - so having it be along one of your larger walls is usually the best approach.

As with your georeference, it's usually a good idea to start from a corner, and aim for another adjacent corner. For example, in our Westport House trace, the first line we used was the following:

<p align="center">
<img src="/images/tutorial/outline_drawing_1.png" width="400" />
</p>

Thanks to the snapping tools, you'll now be able to correctly connect your next line to your base line.
As you can see in the following image, the pink "+" denotes where your new line will connect to the current, with a slightly stronger affinity for the ends.

<p align="center">
<img src="/images/tutorial/outline_snapping_gif.gif" width=600>
</p>

Ensure that the *Enable advanced digitizing tools* button is toggled on, then select the start point of your next line. If it's appropriate (and it *usually* is) you can now use the Advanced Digitizing *Perpendicular* button to create a line at a right-angle to your base line. 
After clicking the button, you simply select the line that you would like to draw perpendicular to. 
When placing the end point of your line, you'll be left with a right-angled corner!

<p align="center">
<img src="/images/tutorial/outline_perpendicular_gif.gif" width=400>
</p>

The button just next to *Perpendicular* is *Parallel* and will, as you might expect, let you select a line that you can draw parallel to.

<p align="center">
<img src="/images/tutorial/outline_parallel_gif.gif" width=400>
</p>

Using the perpendicular digitizing tool to draw our next line leaves us looking something like this:

<p align="center">
<img src="/images/tutorial/outline_drawing_2.png" width="400">
</p>

With all of this in mind, completing _most_ of the outline for Westport House looks something like this:

<p align="center">
<img src="/images/tutorial/outline_drawing_3.png" width="400">
</p>

---

As you can see, there's a section to the bottom right - the curved balcony - that we haven't outlined yet.
We'll cover how to draw curves and circles now.

The way that QGIS draws a circle, or the arc of a circle, is by placing three points and drawing the arc which would connect all three together. Shown in the following examples:

<p align="center">
<img src="/images/tutorial/points_and_curves.png" width="400">
</p>

To create a full circle, you only need two points: your start point, and its antipodal point (i.e. the diametric opposite of the start point). From here, your "third point" is the start point again. This closes the circle.

<p align="center">
<img src="/images/tutorial/full_circle_generation_gif.gif" width=400>
</p>

In the case of Westport House, the floorplan contains a guide showing the centre of the circle. This can be confirmed with the digitizing tools, as we can see the length of the lines (the _d_ field) from centre to edge are equal:

<p align="center">
<img src="/images/tutorial/outline_drawing_4.png" width="400"> <img src="/images/tutorial/curve_distances.png" width="400">
</p>
If you're unsure about how to generate the polygon for your outline, or would like to see an example of the methods that we use internally, you may find [this documentation](https://github.com/wrld3d/wrld-indoor-maps-api/blob/docs_update/A%20-%20Using%20QGIS) helpful.

This means that we can generate a third point, the same distance from the centre, and use that to generate an arc or a full circle.
In the case of our example, we've generated a full circle:
In the case of the Westport House example, our final product looks something like this:

<p align="center">
<img src="/images/tutorial/outline_drawing_5.png" width="400">
<img src="images/tutorial/outline_drawing_8.png" width=250><img src="/images/tutorial/outline_drawing_10.png" width=250><img src="/images/tutorial/outline_drawing_11.png" width=250>
</p>
  
The initial output of this arc is very detailed - the whole circle in our example is 362 verts.

<p align="center">
<img src="/images/tutorial/outline_drawing_6.png" width="400">
</p>

While this level of detail might be desired by some, it's overkill for most circumstances. To remedy this, we can use the *Simplify Feature* <img src="/images/tutorial/buttons/simplify_feature.png"> tool to reduce the vert count to something more manageable.

Simply toggle the tool on, then select the feature that you would like to simplify. The popup window will let you see the before and after of the feature's vert count, and allow you to adjust according to taste.
In this example, we're going to use a value of *0.05* - taking us from 362 verts, to 65.

<p align="center">
<img src="/images/tutorial/outline_drawing_7.png" width="400">
</p>

---

With all of these guides in place, we're now at a point where we can generate our outline polygon for submission!

<p align="center">
<img src="/images/tutorial/outline_drawing_8.png" width="400">
</p>

As before, create a new shapefile layer (*Layer > Create Layer > New Shapefile Layer...*) but this time make sure that the *Type* is set to the *Polygon* radio button. 
In the *New field* section of the window, enter "name" in the *Name* box, ensure that *Type* is set to *Text Data* and that *Length* is 80, and click *Add to fields list*.
Then repeat the above for a "type" text data field.

Your window should now look something like this:

<p align="center">
<img src="/images/tutorial/outline_drawing_9.png" width="400">
</p>

Click *OK*, set your file path and line layer name as you see fit - we're using *building_outline* - and save.
You'll now have a new polygon layer in your project.
To create the actual building polygon you can essentially just "join the dots" of your guide layer.
Select the *Add Feature* tool: <img src="/images/tutorial/buttons/polygon_add_feature.png">

Then just start selecting the appropriate outline points from your guide layer.

<p align="center">
<img src="/images/tutorial/outline_polygon_generation_gif.gif" width="400">
</p>

When you've finished, you should have something like this:

<p align="center">
<img src="/images/tutorial/outline_drawing_10.png" width="400"> <img src="/images/tutorial/outline_drawing_11.png" width="400">
</p>

Congratulations! You're now ready to submit your claim outline to the indoor maps service.
This outline will be the section of the WRLD building data that will be cleared out when you view the interior on our maps, and can serve as the basis for adding all of the interior features that you want to view.