# Changelog

## **2023-01** -> Initial release version

>
>
> Since this is a first release, it may not be free of bugs. Using it within your projects is at your own risk.
> [!BUG]
>
> - Loading presets still buggy, you have to double click multiple times to load preset properly
> [!Main FEATURES]
>
> - Compute shaders, 2D array buffers and 3D are all supported
> - All GLSL TOP parameter types are now supported + all other parameter types with specific syntax, see [[en/Coding with Pixel Wrangle|Coding with Pixel Wrangle]] page for syntax examples
> - All parameter properties are available for customizing appearance, see [[en/Parameters|Parameters]] page for full list
> - Export your shader in a new PanelCOMP in one-click (Removing Pixel Wrangle dependency)
> - UI has been completely rebuild (you can now customize the main background color, text size, text font, text line height), see [[en/index|UI overview]] page
> - Built-in mouse interactions are available everywhere while hovering a viewer and holding [CTRL] key
> - A bunch of macros are added for the most common operations, see [[en/Macros|Macros]] page
> - Add fuzzy search (and path based) to find relevant functions and presets
> - External library auto-parser to use arbitrary pure GLSL library, for td specific path resolving and conversion of old GLSL syntaxes, see [[Functions]]
> - Ability to save your own functions / snippets to build your custom GLSL library
> - Function loader has preview hints (doc header and signatures)
> - Updated keyboard shortcuts, the full list is available in [[en/Keyboard Shortcuts|Keyboard Shortcuts]]
> - Non-default parameters are now always saved with preset (including resolution, bit depth, shader mode, etc...)
