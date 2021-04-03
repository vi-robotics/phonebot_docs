# Electrical Release Guide

In order to make it clear which version of a PCB exactly was manufactured, we have some guidelines on how to export schematics, layouts, and other documents to ensure all relevant documentation for a board revision are up to date.

## Export Schematic PDF

In order to export the PDF of the full schematic, first open the schematic. Then, click "File", then "Plot". In the popup menu that appears, select "PDF" for the output format. For paper size, choose "A4". Choose "Color" for output mode. For the directory, select `PhoneBot/PhonebotBoard/figs`. Leave the rest of the values default. Then click "Plot All Pages". Navigate to the directory, and rename the output file `Schematics.pdf`.

## Export layout PDF

Open the PCB Layout editor. Unfill the copper pours by pressing "Ctrl+B". Next click "File", then "Print". In "Technical Layers" select "F.Adhes", "B.Adhes", "F.Paste", "B.Paste", "F.SilkS", "B.SilkS", "F.Mask", "B.Mask", "Edge.Cuts", and "Margin". Chose "Color" for output mode. Choose "All layers on single page" for pagination. Choose "Fit to page" for Scale. Finally click "Print". Click "Print to File" and choose `PhoneBot/PhoneBotBoard/figs/Layout.pdf` for File. Click "Print". Refill the pours by pressing "b".

## Export 3D View

In the PCB Layout editor, click "View" and then "3D Viewer". Enable Orthographic projection mode. Right click on empty space, and choose "Top View". Click "Zoom to Fit Model", and then press the "Zoom" button as many times as possible before the model exceeds the screen size. Click "Preferences" and "Choose Colors", and then for "Choose Top Color" and "Choose Bottom Color" follow the dialog and pick white for each (255, 255, 255). Next click "Preferneces" and enable ray tracing. Finally, click "File" and "Export Current View as PNG". Name the exported view as "PhoneBotTop.png". Repeat the process for "Right View", "Front View" and "Bottom View", omitting the "Zoom to Fit Model" step and instead just clicking "Zoom" the same number of times as for the top view. Name the files "PhoneBotRight.png" and "PhoneBotFront.png" respectively. These exports help catch major issues with layout, silk screen and component heights.
