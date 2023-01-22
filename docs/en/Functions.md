# Functions

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="100%" height="500px" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Fproto%2FDZoDzaIA6ZhcCWlU0Gr8RC%2FPixel-Wrangle-UI%3Fnode-id%3D294%253A787%26scaling%3Dscale-down%26page-id%3D153%253A40" allowfullscreen></iframe>

## **External libraries**

## Introduction

Pixel Wrangle is designed as a modular environment.

It is therefore possible to extend the available functions through existing GLSL libraries, or by building your own libraries by saving functionality that you can then recall in all your projects.

This can be useful when you want to reuse code more granularly than you could with presets.

Note also that when you import external libraries, they become available throughout your entire project, which means that you can fully reference these imports in a bare GLSL TOP, regardless of the use of Pixel Wrangle.

## Lygia

By default, Pixel Wrangle provides a very complete granular function library called [Lygia](https://lygia.xyz).

[Lygia](https://lygia.xyz) is a community project led by Patricio Gonzalez Vivo, who is also behind  <https://thebookofshaders.com>, an introduction to fragment shaders.

For more information on this library and its content, you can also consult the [Official GitHub](https://github.com/patriciogonzalezvivo/lygia)

## Important files and folders

Files related to external libraries are stored in your USER Functions folder, you can open it by clicking on the bottom right corner folder in Pixel Wrangle UI.

To add an external library of your choice, you can clone its content into the Functions/**\<Library Name>** folder.

In the following sections, you will see how to integrate these libraries into your projects.

I also specify that for the library to be readable by Pixel Wrangle, the files must have a '.glsl' extension, any other type of file will be ignored.

## Auto-parsing

Pixel Wrangle provides a summary parsing tool that convert pure GLSL libraries compatible GLSL libraries with the Touchdesigner ecosystem.

This mainly concerns the form of the import paths that are specific to Touchdesigner, but also the conversion of certain function names such as texture2D(), and others so that the library runs correctly in the Touchdesigner environment.

The principle is as follows:

- You drop your raw library folders in your USER Functions folder as described above
You parse your libraries directly from the Pixel Wrangle interface (Just press 'Import Libraries' from the 'Code/Plugin' page)
Once the libraries are parsed, an automatically Touchdesigner-compatible copy of the file / folder architecture is made in the 'Functions/dist' folder.

Then, the entire file/folder architecture at the root of your TouchDesigner project, in a BaseCOMP called 'libs'.

Each '.glsl' import is actually a TextDAT in Sync File mode, which points to the corresponding file in the Functions/dist folder (the copy of the file after parsing).

Since the library architecture is integrated into the root of your project, you can reuse all functions in a GLSL TOP context without going through the Pixel Wrangle envelope.

## Including Functions in Your Pixel Wrangle Instance

By pressing the shortcut [CTRL] + [SHIFT] + [TAB], or clicking in the search bar at the bottom right of the interface, you can access the function explorer.
This explorer returns a list of all functions available within your project.
You can then search by typing a keyword, the list is automatically filtered.

Pixel Wrangle will also provide header documentation for all your functions, as well as all the signatures that you can copy and paste directly into your code.

Finally, you can double-click on the function in the list, which will import it into your Pixel Wrangle instance. You can see the inclusion appear in your 'Functions' panel.

## Adding your own functions

If you want to add new functions as you use Pixel Wrangle, you can do so using the '+' function in the Functions panel.

You will then be asked for the name of the file to save as well as a header comment to provide useful information about the role of the file and its functions.

By default, these presets are saved to the root of your Functions/user folder in a '.glsl' file. You can then place them in the directories of your choice and define the hierarchy of your choice in your project.

## Organize your functions into a useful library

There is no particular method for creating your own GLSL function libraries, but some recommendations:

- Try to avoid creating dependencies on functions from other libraries to preserve portability of your library in different contexts
- As much as possible, try to organize your function files granularly and adopt a hierarchy by utility / context for clarity
- With the exception of overloading functions or declaring variations of signatures for the same function, it is recommended to have only one functionality per file.
