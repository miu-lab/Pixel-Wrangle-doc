# Visual Studio Code

## Introduction

Pixel Wrangle offers the ability to edit your shader code within Visual Studio Code by default.

Visual Studio Code is a very popular code editor offered by Microsoft, it is free and has a wide extension ecosystem.

For more information, you can visit the [official Visual Studio Code website](https://code.visualstudio.com).

## Configuration

To take advantage of the Visual Studio Code integration within Pixel Wrangle, you must first set the path to the Visual Studio Code executable.

By default it is located:

- Either in your personal folder (if you are doing an installation for your user account only) => **C:\Users\<YOUR-USER-ACCOUNT-NAME>\AppData\Local\Programs\Microsoft VS Code\Code.exe**

- Or in your system partition (if you are doing an installation for all users -> requires administrator rights) => **C:\ProgramData\Microsoft VS Code\Code.exe**
To set the path to Visual Studio Code, you have two options:

- Through the Pixel Wrangle interface: (If the parameter is locked, you can right-click and then toggle read-only to enable 'Edit' mode)
![[vs-code-path-from-parameters.png]]

- Through the settings.json file located in the root folder of the project:
![[vs-code-path-from-json.png]]

Once this is done, you can launch your code editor in Visual Studio Code by pressing the corresponding button in the UI, or by pressing **[CTRL] + E** in the UI. This path will then be common to all your instances of Pixel Wrangle in all your projects.

## Environment

By default, the first time you launch Visual Studio Code through Pixel Wrangle, a new Visual Studio Code workspace is created. It will be used every time you open Visual Studio Code through a Pixel Wrangle instance.

The goal is to not interfere with your existing Visual Studio Code environments and allow you to customize it to your liking (shortcuts, extensions, etc).

Additionally, this environment will be preserved through updates of Pixel Wrangle as it is stored locally on your machine and remains independent of the Pixel Wrangle installation. By default, the location of this environment is: **C:\Users\<YOUR-USER-ACCOUNT-NAME>\.vscode-td\glsl**

## Location of Pixel Wrangle's Temporary Files

When writing code in Pixel Wrangle, each time a modification is made, the files are saved to the hard drive. The location of these files is in the general Touchdesigner temporary folder, which is typically located in:

## Provided .code-profile

To start with, Pixel Wrangle offers a Visual Studio Code profile that includes a set of extensions, keyboard shortcuts. This configuration is intentionally minimalist and tailored for writing GLSL. In addition, some specific variables for the GLSL environment in Touchdesigner are injected, which allows for better autocompletion without having to perform imports.

You can directly load this profile in Visual Studio Code using the command palette ([CTRL] + [SHIFT] + P), and run the command: 'Import Settings Profile'

![[vs-code-import-profile.png|600]]

If you want to load this profile file into your environment, you can find it at the root of the Pixel Wrangle project in your Palette

![[vscode-profile-path.png|600]]
