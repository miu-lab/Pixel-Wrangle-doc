# Install

## Prerequisites

The only thing you definitely need to get started is a **recent version of Touchdesigner (builds 2022.XX and later)** installed on your machine.

**If you don't have a license, you can still use Touchdesigner with a 'NON-COMMERCIAL' license**. These versions have the vast majority of features and are currently capable of running Pixel-Wrangle.

**Touchdesigner can be downloaded by following this [link](https://derivative.ca/download)**, it will give you an overview of the different license models.

Optionally, you can [download Git](https://git-scm.com/downloads) if you want to update future versions of Pixel Wrangle directly through your Terminal.

Finally, if you want to take advantage of the documentation (in Markdown) in the best conditions, you can [download Obsidian](https://obsidian.md/). You will have access to it independently of your internet connection with a nice interface.

## Downloading the Project

Once Touchdesigner is installed,

### **If you have not installed Git**

- Download the repository as a ZIP file from <https://github.com/miu-lab/TD-Pixel-Wrangle> by clicking on 'Download ZIP'
![[install-download.png]]

## Installing Pixel Wrangle

### **If you have not installed Git:**

- Move the downloaded zip file to **C:\Users\<YOUR-USER-ACCOUNT-NAME>\Documents\Derivative\Palette** and extract its contents to this folder as shown below

- **Attention, it is important that the root folder of the project has the exact name: TD-Pixel-Wrangle**

![[install-path.png]]

### **OR If you have installed Git**

Launch a terminal in the folder **C:\Users\<YOUR-USER-ACCOUNT-NAME>\Documents\Derivative\Palette** or navigate there with cd and then do a 'git clone' of the project

```bash
git clone https://github.com/miu-lab/TD-Pixel-Wrangle.git
```

## Configuration

If you wish to work with Visual Studio Code, you can immediately specify the path to the executable in the 'settings.json' file at the root of the project.

```json
{"vscodePath": "C:\\Users\\<YOUR-USER-ACCOUNT-NAME>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"}
```

Pour plus de détails sur l'utilisation conjointe de Pixel Wrangle avec Visual Studio Code, rendez-vous sur la page [[Visual Studio Code]]

## Documentation

Si vous souhaitez accéder à la documentation en local et dans les meilleures conditions comme évoqué plus tôt, vous pouvez [télécharger Obsidian](https://obsidian.md).

You can then open a "vault" (vault: Obsidian terminology for designating a file and folder architecture in markdown).

The documentation vault is stored in the 'doc' folder at the root of the project, you can open it directly in Obsidian
Path: **C:\\Users\\<*YOUR-USER-ACCOUNT-NAME*>\\Documents\\Derivative\\Palette\\TD-Pixel-Wrangle\\doc**

## Updating the Touchdesigner Palette

After following these steps, it may be necessary to refresh the Touchdesigner Palette when launching Touchdesigner.
![[install-refresh-palette.png]]
