# Presets

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="100%" height="500px" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Ffile%2FDZoDzaIA6ZhcCWlU0Gr8RC%2FPixel-Wrangle-UI%3Fnode-id%3D153%253A10%26t%3DuJ7kUBcROLYzKe7F-1" allowfullscreen></iframe>

## Generalities

Pixel Wrangle has a system for managing presets.

A Pixel-Wrangle preset contains both all the code you have written in each of the shader panels, as well as all the parameters and their state at the time of saving.

To make it easier to load and filter presets, Pixel Wrangle provides a basic explorer with a file / folder hierarchy that reflects the structure of your file system

The explorer allows you to find, load or save presets.

It can be activated by clicking on the search area located at the bottom left of the interface or via the shortcut [CTRL] + Tab.

You can then type in the name of your search to filter the results or scroll through the architecture.

## Important paths

Presets are stored by default in the Presets folder at the root of Pixel Wrangle in your Palette.  

There are 2 main roots:

- Factory library (\<Pixel Wrangle plugin root>/Presets) : This is the folder of presets that are shipped with Pixel Wrangle (Read-only)
- User library (USER/Presets): This is the folder where your presets are saved, open this folder with the folder icon at the bottom left corner of Pixel Wrangle UI or CTRL + O

All the created subfolders are reflected in the Pixel Wrangle preset browser.

## Load a preset

To load a preset, simply double-click or press enter

**Warning, sometimes loading can be erratic, you may have to double-click several times.
This is a known bug that should be fixed in upcoming versions of Pixel Wrangle**

## Save a preset

To save the state of your shader in a new preset, you can click on '+' in the UI. This will open a pop-up that will ask you under what name you want to save this preset. By default, this preset will be saved in the USER/Presets folder.

You can also use path based naming schemes :

- gen/noise/mycrazynoise will save a preset called 'mycrazynoise.json' and create all the intermediates folder from the path selected in the left side. If nothing is selected, it will start from the USER/Presets (root)
