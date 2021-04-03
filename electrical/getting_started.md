# Electrical Getting Started

Before you get started assembling the PhoneBot board, you will need to have one manufactured. Luckily there are many manufacturers who will build PCBs at a relatively low cost, and you can easily order the PhoneBot PCBs from them. The electrical build process consists of four main parts:

1. Ordering the PCBs
2. Ordering the components
3. Assembly
4. Testing

## Ordering PCBs

In order to order the PhoneBot PCB from a manufacturer, you'll need to export the manufacturing files. First, open the project in KiCAD by navigating your local clone of [phonebot_board](https://github.com/vi-robotics/phonebot_board). Then open the PCB editor, where you can open `File/Plot` to export the design files. To figure out what settings to use, try to find the guidelines from your board manufacturer (for example look at [PCBWay's export instructions](https://www.pcbway.com/blog/help_center/Generate_Gerber_file_from_Kicad.html)).

There are three unique boards you'll need to order.

- 1x main board
- 4x A servo board
- 4x B servo board

A stencil can make soldering the componennts much easier, and is recommended if not cost prohibitive.

## Ordering Components

For each of the boards, open the schematic in KiCAD and export the BOM. There are various BOM generation tools which will create a CSV from the schematic. The field `Part` is populated with the URL of the part to order. It is usually advised to order slightly more components than the board requires in case of component failure, poor soldering, losing components etc.

## Assembly

The PhoneBot main PCB is mostly surface-mount soldered, and thus requires either a rework station or reflow oven. There are many good tutorials on how to surface mount solder.
