
### Adobe Animate CC

You can export your Adobe Animate CC Scenes directly for the Reality Editor.
		
1. File > Publish Settings > Output File > Chose the directory of your Hybrid Object and add "index.html"
2. File > Publish for publishing your Hybrid Object UI

For a transparent Background you should add the following action in the first frame:

	canvas.style.backgroundColor="rgba(0,0,0,0)";
	document.body.style.backgroundColor = "rgba(0,0,0,0)";	
	

### Adobe Edge Animate

1. Name your project "index".
2. File > Publish Settings > Target Directory > Chose the directory of your Hybrid Object
3. File > Publish for publishing your Hybrid Object UI

### Adobe DreamWeaver

1. Make sure that all sizes are absolute. This means that the width and height are in px and not in %.
1. Save your project as index.html


### Adobe Muse

1. When creating a new Site chose **"Fixed Width"** 
2. Export as HTML and chose the folder of your Hybrid Object

For transparent Background:
Page > Page Properties > Metadata > HTML for <head> > Past the following code in to the Text Area:

	<style>
		.html {
			background:transparent !important;
		}
	</style>
	
### Adobe InDesign

File > Export > index.html
