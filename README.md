# sprintFont manual
sprintFont is a plugin for Sprint-Layout v6 2021 and newer version.   

for version: v1.4



## 1. Features
* Insert text in other fonts
* Import the footprint from Kicad/EasyEDA
* Insert the SVG image
* Insert Qrcode
* Supports auto-routing
* Add teardrop pads




## 2. Usage



### 2.1 Install plugin
1. Decompress sprintFont to a directory, open Sprint-Layout v6.0 2021 and newer version, click menu ["Extras" / "Define Plugin"]

![define_plugin](pic/define_plugin.png)



2. Locate the sprintFont.exe

![plugin_path](pic/plugin_path.png)



3. Execute the plugin by clicking menu ["Extras" / "Run Plugin"]

![define_plugin](pic/define_plugin.png)





### 2.2 Insert text in other fonts



1. Switch to the "Font" page

Choose a font and change some parameters for your application.

![main](pic/font_main.png)



2. Return to Sprint-Layout by clicking "OK", the text you entered will be "sticky" on the mouse, move to the desired position and click the mouse to drop it.
If the layer "C2 (Back copper)" or "S2 (Back silkscreen)" is selected, the font is automatically mirrored horizontally.

![dual_silk](pic/dual_silk.png)



3. For unicode symbols in font file, you can digit "\u1234" to insert it into Sprint-Layout (replace 1234 with the unicode code of symbol)







### 2.3 Import footprint

<span style="color: red;font-weight: bold;">Disclaimer: I am not responsible for any loss caused by the incorrect package imported by this plugin. If you do not agree with this disclaimer, please stop using this plugin immediately.</span>



1. **Switch to the "Footprint" page**

![footprint_main](pic/footprint_main.png)



2. **Import from Kicad**

Kicad installer is already packaged with a lot of footprint libraries. If you don't want to install Kicad, you can just download the libraries from this link [Kicad official libraries](https://gitlab.com/kicad/libraries/kicad-footprints), in addition, many component search websites also provide footprint library in Kicad format, such as [Component Search Engine](https://componentsearchengine.com).    



**Steps:**    
Click the button on the right side of the text box to select the footprint kicad_mod file in your machine to import to Sprint-Layout. It is compatible with Kicad_v5 and Kicad_v6 format.     

![kicad_footprint](pic/kicad_footprint.png)



![kicad_imported](pic/kicad_imported.png)



3. **Import from EasyEDA**


**Steps:**    
If you want to import the footprint from EasyEDA, the first thing is to find the LCSC part code of the component, you can go to the website [EasyEDA site](https://easyeda.com/editor), Click the "Library" in the left navigation bar, search and pick the LCSC part code at the bottom of the page     

![easyeda_find_code](pic/easyeda_find_code.png)



Digit the LCSC code in the text box, press Enter or click the "Ok" button to import it.    
![easyeda_imported](pic/easyeda_imported.png)






### 2.4 Insert SVG images/Qrcode

This plugin also supports the importation of SVG vector graphics, but it does not implement all SVG commands internally, so it can only support simple graphics, such as LOGO. 

![svg_main](pic/svg_main.png)




![svg_imported](pic/svg_imported.png)






### 2.5 Autorouting

This plugin successfully added auto-routing functionality to Sprint-Layout.    
The solution is the same as Kicad, divided into three steps:
1. Export the board to Specctra DSN format.   
2. Use the open source auto-router [Freerouting](https://github.com/freerouting/freerouting/releases) to do auto-routing work and save routing results as a SES file.   
3. Import the SES file back to Sprint-Layout.   




#### 2.5.1 Usage



2.5.1.1 **Export to Specctra DSN format**

1. Switch to the layer "O" firstly in Sprint-Layout, and define a closed zone as board boundary, which can be of different shapes such as rectangles, circles or irregular shapes. After that, switch to other board layers to place components and layout them appropriately. Use the "Connections" tool to connect the pins that need to be connected, this connection is called Ratsnest or Airwire or other names in different software.   

![airwire_and_ulayer](pic/airwire_and_ulayer.png)



2. Deselect all items in Sprint-Layout (<span style="font-weight:bold;color:red;">no components or tracks can be selected</span>), run the plugin, switch to the "Autorouter" page.   

![autorouter_main](pic/autorouter_main.png)



3. Specify the DSN file name, modify the rule item value by double-clicking the row, and click "Export DSN" to export the DSN file.   
<span style="color:red">This plugin also generates a pickle file with the same name of DSN file, please do not delete it, this file will be used when importing SES</span>



2.5.1.2 **Auto-routing**

1. [Download and install Freerouting](https://github.com/freerouting/freerouting/releases), open it and load the DSN file.


2. Click "Autorouter" on the toolbar and wait for it to complete the routing. If the circuit board is complex, it may take a long time to run.    

![freerouting_main](pic/freerouting_main.png)



3. The default configuration is for double-sided board, it means both the top and bottom copper layer are allowed to place tracks. If single-sided copper layer is required, you can select the copper layer you need through the Freerouting menu ["Parameter" / "Autoroute"] dialog

![freerouting_layers](pic/freerouting_layers.png)



4. After the routing is completed, save the result as an SES file via the menu ["File" / "Export Specctra Session File"]

![freerouting_save_ses](pic/freerouting_save_ses.png)



2.5.1.3 **Import SES to Sprint-Layout**

1. Select the correct SES file (ensure that the pickle file with the same name exists), click "Import SES" to directly import the routing result into Sprint-Layout. Sprint-Layout does not necessarily need to pre-open the previous board, it can be a blank board.

2. Hold Shift and click "Import SES" to display a menu with more import options.

![import_ses_options](pic/import_ses_options.png)

* **Import all (remove routed ratsnests)**: It's default behavior when you click "Import SES". The airwires with copper connection are deleted, and the airwires without copper connection are retained.
* **Import all (remove all ratsnests)**: Import all the routing results and components and replace all components on the board, and delete all airwires.
* **Import all (keep all ratsnests)**: Import all the routing results and components and replace all components on the board, and keep all airwires.
* **Import auto-routed tracks only**: Import tracks only, do not import components, do not delete any elements on the circuit board, the imported tracks will "stick" to the mouse, and move to the correct position to drop.




#### 2.5.2 other details for auto-routing
* If there is a .rules file in the same folder of the DSN file, Freerouting will use this file to overwrite the routing rules in the DSN file, so maybe need to delete the .rules file if the result is not what you set in plugin.
* Components can only be placed on the front side. for SMD component, both the component body and the pads are on the front side. for THT component, the component body is on the front side, and the single-sided pad is on the back side, can be any side if is THT (plated) pads. (This is default behavior when you place a component in Sprint-Layout)
* If there are some areas that cannot be routed, you can draw a polygon and set it as "Cutout-area". Or draw it on the layer O (EdgeCuts) can achieve the same effect, but the polygons in the layer O will affect the final circuit board shape.
* If you use the "Disintegrate Component" function to modify the component's pad or silk screen, you have to convert the group back to component again ([right click "Build group" -> right click "Component"]), otherwise Freerouting only displays pads, not silkscreens. (but silkscreens are not lost, they will appear again when be imported into Sprint-Layout)
* Some critical tracks such as power/clock can be prerouted, or modify them manually after Freerouting finished. Auto-routing can be used as the starting point of routing, and can also be used as the end point of routing.
* Due to the limitation(bug?) of Freerouting, silkscreen lines can only be horizontal and vertical or 45-degree. Other angles will be incorrectly drawn in Freerouting, but silkscreen does not affect routing.
* Due to the limitation of Freerouting, the arc of the silk screen is not drawn.
* Sometimes when SES file been imported, the routed net-connections (Ratsnest) have not been deleted. This is a bug of Sprint-Layout. Just create a new blank board and import it again, the problem can be solved.





### 2.6 Teardrop pads

The algorithm of teardrop pads is from <https://github.com/NilujePerchut/kicad_scripts>, thanks in advance.

![teardrop_main](pic/teardrop_main.png)




#### 2.6.1 Basic operation
1. If deselect all elements in Sprint-Layout before executing this plugin, teardrop will be applied to all THT pads. If only some pads need to be added teardrops, you can select the both pads and tracks you need firstly. Deleting teardrops is the same logical, you can delete all teardrops or only those in the selected region.
2. By the legend in the GUI, it should be easier to understand the meaning of the three parameters. The base of the percentage is the outer diameter of the pad.

![teardrops](pic/teardrops.png)



#### 2.6.2 Details
* thermal pads will not be processed.
* If the teardrop parameters are the same, the teardrop pad will not be added repeatedly. but if the parameters are different, multiple operations may add some overlapping teardrop pads.





## 3. Changelog



### v1.4
* Added teardrop pads



### v1.3
* Added auto-routing (use Freerouting as auto-router)
* Added support for ttc/otc font format
* Some minor optimizations



### v1.2
* Insert Qrcode



### v1.1
* Import footprint from Kicad/EasyEDA
* Insert svg image



### v1.0
* Insert text in other fonts into Sprint-Layout



