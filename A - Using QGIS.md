Using QGIS
===================

1. [Creating An Indoor Map Level](#creating-features)
1. [Exporting Your Level In GeoJSON Format](#export-level-to-geojson)
1. [Creating the main.json File](#create-json-file)
1. [Packaging Your Map For Submission](#create-package)
1. [Submission of the Map to the WRLD Indoor Maps API](#submit-package)

---

#### <a name="creating-features"/>Creating Features in QGIS

To generate a polygon feature for our interior, we first start by creating a line layer via *Layer > Create Layer > New Shapefile Layer...*. This will serve as our "scratch" layer for generating our guiding lines - think of it like a sketch before you draw the main polygons.

On the New Shapefile Layer popup window, select the *Line* radio button, ensure that your *CRS* is set to *3857* (unless you're specifically using a different CRS) then hit OK.
Set your file path and line layer name as you see fit - e.g. something like "scratch" or "guide" - and confirm.
Your new line layer should now be present in the *Layers* panel on the left of the window.
Click and drag this layer to the top of the list, to ensure that it will be visible over our other layers.
Using *Right click > Properties > Style* you can change the colour of this layer to make sure it's visible - bright red or green tend to work well.

######Lines and Digitization

With your scratch layer set up, you can begin to draw the guide lines for your outline's perimeter.

- Click the “Toggle Editing” button 
![Toggle editing button](/images/tutorial/edit_toggle.png)

- Click the “Add Features” button 
![Add features button](/images/tutorial/line_add_feature.png)

- We also use the *Snapping Tool* and *Advanced Digitizing Panel* to ensure that we can make points accurate, and keep walls parallel and perpendicular to one another, where appropriate.
- Set the snapping parameters via *Settings > Snapping Options...*
- More often that not, our settings are something similar to the following: 
![Snapping Panel](/images/tutorial/snapping_panel.png)

- These settings ensure that your can snap to any corner or edge on any active layer, including those that you're not currently working on (which will be important later).

- Ensure that Advanced Digitizing is active via *View > Toolbars > Advanced Digitizing Toolbar*
- You may notice that there's a new button available while your layer is in edit mode: the *Enable advanced digitizing tools* button.
![Digitizing Toggle](/images/tutorial/buttons/digitization_toggle.png)

It's useful to create a "base line" if you don't have one already - i.e. a line that you can use as a reference for keeping your walls and rooms parallel and perpendicular to one another. With this in mind, it's usually best to have this base line be along one of your larger walls.

As with georeferencing, it's usually a good idea to start from a corner, and aim for another adjacent corner. For example, in our Westport House trace, the first line we used was the following:
![Base Line Drawn](/images/tutorial/outline_drawing_1.png)

Thanks to the snapping tools, you'll now be able to correctly connect your next line to your base line.
In the following image you can see the pink "+" which denotes where your new line will connect to a previously placed line, with a slightly stronger affinity for the ends.
![Outline Snapping](/images/tutorial/outline_snapping_video.mp4)

Ensure that the *Enable advanced digitizing tools* button is toggled on, then select the start point of your next line. If it's appropriate (and it *usually* is) you can now use the Advanced Digitizing *Perpendicular* ![Perpendicular Button](/images/tutorial/buttons/perpendicular_line.png) button to create a line at a right-angle to your base line. 
After clicking the button, you simply select the line that you would like to draw perpendicular to. 
When placing the end point of your line, you'll be left with a right-angled corner!
![Perpendicular Outline Edge](/images/tutorial/outline_perpendicular_video.mp4)

The button just next to *Perpendicular* is *Parallel* ![Parallel Button](/images/tutorial/buttons/parallel_line.png)and will, as you might expect, let you select a that you will draw parallel to.
![Parallel Outline Edge](/images/tutorial/outline_parallel_video.mp4)

Using the perpendicular digitizing tool to draw our next line leaves us looking something like this:
![Base Line Drawn](/images/tutorial/outline_drawing_2.png)

As an example of putting all of this into action, we can create most of the outline for Westport House, which looks something like this:
![Outline Majority Complete](/images/tutorial/outline_drawing_3.png)

---

######Arcs and Curves

As you can see, there's a small section to the bottom right - the curved balcony - that we haven't covered yet.
<br>We'll talk about how to draw curves and circles now.

The way that QGIS draws a circle, or the arc of a circle, is by placing three points, and drawing the arc which would connect all three of these points. Shown in the following examples:
![Arc Generation](/images/tutorial/points_and_curves.png)

To follow on from that, the creation of a full circle only really needs two points: your start point, and its antipodal point (i.e. the diametric opposite of the start point). From here, your "third point" is the start point again. This closes the circle.
![Circle Generation](/images/tutorial/full_circle_generation.mp4)

In the case of Westport House example, the floor plan actually contains a guide showing the centre of the circle. This can be confirmed with the digitizing tools, as we can see the length of the lines (the _d_ field) from centre to edge are equal:
![Balcony Floor Plan](/images/tutorial/outline_drawing_4.png) 
![Curve Distances](/images/tutorial/curve_distances.png)

This means that we can generate a third point, the same distance from the centre, and use that to generate an arc or a full circle.
In the case of our example, we've generated a full circle:
![Balcony Circle](/images/tutorial/outline_drawing_5.png)

The initial output of this arc is very detailed - the whole circle in our example is **362** nodes.
![So Much Detail](/images/tutorial/outline_drawing_6.png)

While this level of detail might be desired by some, we find that it's overkill for most circumstances, and in extreme cases can lead to poor performance while within the interior. 
<br>To remedy this, we can use the *Simplify Feature* tool to reduce the node count to something more manageable.
![So Much Detail](/images/tutorial/buttons/simplify_feature.png)

Simply toggle the tool on, then select the feature that you would like to simplify. The popup window will let you see the before and after of your node count, and allow you to adjust according to taste.
In this example, we're going to use a value of *0.05* - taking us from 362 nodes, to 65.
![That's Better](/images/tutorial/outline_drawing_7.png)

---

######Polygon Creation

With all of these guides in place, we're now at a point where we can generate our outline polygon for submission!
![Ready To Go!](/images/tutorial/outline_drawing_8.png)

As before, create a new shapefile layer (*Layer > Create Layer > New Shapefile Layer...*) but this time make sure that the *Type* is set to the *Polygon* radio button. 
In the *New field* section of the window, enter "name" in the *Name* box, ensure that *Type* is set to *Text Data* and that *Length* is 80, and click *Add to fields list*.
Then repeat the above for a "type" text data field.

Your window should now look something like this:
![New Polygon Shapefile](/images/tutorial/outline_drawing_9.png)

Click *OK*, set your file path and line layer name as you see fit - we're using *building_outline* - and save.
You'll now have a new polygon layer in your project.
To create the actual building polygon you can essentially just "join the dots" of your guide layer.
Select the *Add Feature* tool: 
![Add Polygon Feature](/images/tutorial/buttons/polygon_add_feature.png)

Then just start selecting the appropriate outline points from your guide layer.
![Outline Polygon Generation](/images/tutorial/outline_polygon_generation_vid.mp4)

When you've finished, you should have something like this:
![Full Outline Polygon](/images/tutorial/outline_drawing_10.png)
![Full Outline Polygon](/images/tutorial/outline_drawing_11.png)

---

While this example has been related to the generation of a large polygon - the Westport House outline - the information on this page should be relatable to most of the polygons that you'll need to make in order to complete your building.

---

In order to submit the building outline, we first need to export it as a geojson file, rather than a shapefile.
In doing this, we can also convert the format from *Pseudo-Mercator* to *Mercator* and have the points stored as latlongs - which the WRLD system will use to convert the submission into a 3D model.

#### <a name="export-level-to-geojson"/>Exporting the Level to GeoJSON

- Highlight the polygon layer that you created in the *Layers* panel
- *Right click > Save As…*
- Set *Format* to GeoJSON
- Change the *CRS* setting to WGS 84 ([EPSG: 4326](http://spatialreference.org/ref/epsg/4326/))
- Ensure that *Encoding* is set to *UTF-8*
- Set your file name and location
- Click *OK*

#### <a name="submitting-outline"/>Submitting the Outline to WRLD

To submit to our REST API via a curl command, you'll need your WRLD developer token (which you can find [here](https://accounts.wrld3d.com/users/edit))

You'll then need to fill in the following information, for everything contained within a pair of <>, as appropriate to your submission:

```sh
$ curl -v -XPOST https://indoor-maps-api.wrld3d.com/v1/edits/?token=<my developer token> -F name="<my venue name>" -F venue_street_address="<my venue address>" -F venue_phone_number="<my venue phone no.>" -F venue_email="<my venue owner email address>" -F submission_contact_email="<my email address for notifications>" -F venue_outline="@</path/to/my/file>.geojson"
```

On successful completion of this request, you should receive a reply email which contains a 40-character [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) in the format *EIM-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*. Please make note of this UUID, as you'll need it for any future updates or submissions to your indoor map.

---

The indoor maps API will now validate your outline and move your edit from the *AwaitingApproval* state to the *ApprovedForSubmission* state.  This should happen within a few minutes.  You will be e-mailed if any problems arise during this process.  Alternatively, you can also query the status of the edit with the following command (you’ll need to replace the UUID shown here with the one that was returned by your initial POST request):
```sh
$ curl -v https://indoor-maps-api.wrld3d.com/v1/edits/<my UUID>/status?token=<my developer token>
```
Once the approval process has run, you should see the following within the query response:
```json
{"status":"ApprovedForSubmission"}
```

If you have any problems, the [cheatsheet](CHEATSHEET.md) might be able to help, or feel free to [raise an issue](https://github.com/wrld3d/indoor-maps-api/issues/new) or get in touch with us at support@wrld3d.com.