## ICSE [ Illustrator circuit -> Silhouette Studio AppleScript plugin v0.1a ]

This repo is dedicated to the Illustrator Circuit Silhouette Export - plugin/extension to Adobe Illustrator, written in AppleScript.

This tool aims to prepare files to be sent to the Silhouette Portrait cutting tool.

When processing a file, it manipulates the content of layers whose names starts with specific string of chars, and handles the necessary reworks:
- replacing every path with a fill color ( considered "shape"s ) with a grid of lines through a pathfinding op
- applying an offset path op on every path with no fill color but a stroke color ( considered "lines" )
- converting texts to said-"shapes", by getting their outline & then processing them like the paths with a fill color

Once the processing of the file is done, a directory is created in the same location, named "<your_illu_file>_ICSE", and populated with the resulting files, in their own subdirectory ( .xdf, and .ai files as well if specified )

The .xdf files exported are ready to be "printed" & require no further scaling to respect the original aspect ratio & Cie


Please bear in mind that the tool is in a very early stage & may not be optimised as could be ( .. )


For more infos & the reason for all this, see below

-------

#### Summary

This tool aims to simplify & streamline the process of "printing" circuits on the Silhouette Portrait ( & maybe, but untested, other Silhouette devices / other devices as well )

I know that a tool called "Silhouette Connect" exist, but for some reason, it doesn't work for me ( at least, on Mac OS X 10.6.8 & Adobe Illustraotr CS5 ).

This aside, I have to say I have a really hard time with the Silhouette Studio app ( that's buggy as hell for some reason(s) .. ), particularly in the way it auto resizes stuff &

doesn't provide accurate controls ( to my subjective, all personal taste ) to recover from that said f****n' auto-resize stuff :/ ..

A little trick I found, if someone were to read these lines, is to override the units while exporting as a .dxf, so that 1mm ( or whatever unit you're working with ) => 0.03935 unit(s)

( in case you're wondering, yes, this is the closest value we can use to get a "~perfect" aspect ratio between Illustrator & Silhouette Studio, yes it is precise enough for my ruler,

and lastly, yes, I did get this "magic value" by fail & retry using a custom so-called "calibration sheet", which by the way includes more and more stuff, like traces for the ATmega328P, the HC-05, ..

-> handy for a quick print & check )



-------

#### On the hardware side of things

I'm currently using:

- a [ not modified .. yet ? ] Silhouette Portrait

- a bic (ballpoint) pen for the labels

- a "circuit scribe conductive ink" pen for the traces ( I plan to mix my own conductive ink as soon as I get the elements & time to ;p )

- maybe a marker for the traces cover, but untested yet ( my guess is ink is basically not conductive, but I didn't test having two traces separated by only a thin layer ink .. and "printed" one right after the other .. :| )

-------

#### For each "pcb plate" 

( actually, each "[thin] [transparent] sheet of whatever" ), we may have the following layers, which all follows a naming convention:

- "pcb_silkscreen"  => for the labels & drawings

- "pcb_soldermask"  => for the traces cover, in non-conductive ink

- "pcb_ratsnest"    => for the traces, in conductive ink

- "pcb_solderpaste" => for the SMD/SMT pads, to get solder paste ( or conductive paint ? )

- "pcb_drill"       => for the through-hole components & the "~vias" ( actually, just holes filled with conductive paint ? ), to be cut by .. the cutting tool !

- "pcb_fold"        => half-depth cuts, to ease folding parts of a plate has such weirdness ;p

// your contribution ? ;)

- "pcb_"            =>

-------

#### For the plugin to work correctly & a successful parsing

The content of the above layers has to be properly formatted ( if using 123D Circuits -> Illustrator plugin, it should be up & ready, already ;p )

All the layers are subject to the following, whatever their "type":

- paths with no "fill color" but a "stroke color" are considered "lines" => if their "stroke width" is greater than 1pt, "offset path" operations are executed to render a correct/corresponding stroke width

- paths with a "fill color" are considered "shapes" => they get replaced by the neccesary amount of lines of 1pt stroke width, grouped, on which a "pathfinder outline" operation is executed to render the original shape as lines

- texts => they get converted to their outline, and then the path obtained is processed as the above "shape"

For images ( not vectors ), one could script a "live tracing opts" > "default" > "expand" operation; & doing so prepare the miage as a correct "shape"

-------

#### Workflow

- get an Illustrator file containing a circuit ( download it, draw it, or use the "123DCircuitDL" & "123D Circuits -> Illustrator AppleScript plugin" to get a clean .ai from a .svg containing a circuit's pcb )

- have in it ( at least ) layers named "pcb_silkscreen" & "pcb_ratsnest", which respectively contains the labels & drawings of the boards and the footprints & conductive traces ( if the project you're working on requires additional layers, see above for the ones already handled, and feel free to init a push with your mods, I'll be happy to add them ;p )

- for "boards" that needs multiple layers, the idea is to use the "pcb_drill"-prefixed layers to have holes where some conductive material can be put, to make a ~"via" ( on the same subject, boars with multiple layers can either use the "pcb_soldermask" layer & a non-conductive material to cover the traces of each "pcb_ratsnest" related layer, use an in-between layer for each layer, or mirror the traces of some layers to have them printed on the back of the support, .. )

- once ready, launch the script ( from Illustrator or directly ( .. ) )

- on completion, the mac 'll beep ( at least for now, but it could do plenty of other stuff as well .. )

- now, open the necessary files in Silhouette Studio & adjust the "cutting tool" settings for each of them before hitting the "send to <device>" button ( do othe adjustements if you wish, but none should be required )

- choice is left up to the Maker on deciding to change the "cutting tool" used for each layer's "printing" and/or the support as well

-------

#### Remember other closely related useful tool(s):

123DCircuitDL -> javascript plugin that spawn an export pane in the UI that allows to either download a view by clicking on it or download the selected ones contained within a .zip file

123D Circuits -> Illustrator AppleScript plugin => to "cleanup" a circuit coming from Autodesk's 123D Circuits & exported using the "123DCircuitsDL" tool

Illustrator circuit -> Silhouette Studio AppleScript plugin => to export all correctly named layers in an Illustrator file as properly scaled/parsed .dxf files, ready to roll

-------

#### Contribute

Feeling like you can help on the project ? there's so much to be done ! ^^

- diggin' a way to have "\_/" traces in Illustrator
- diggin' a way to generate a ratsnest in Illustrator
- diggin' a way to have automated "cutting tool" change on the Silhouette device
- diggin' an alternative to the Silhouette Studio GUI that'd allow above change & sending settings to the Silhouette device
- ( .. )
