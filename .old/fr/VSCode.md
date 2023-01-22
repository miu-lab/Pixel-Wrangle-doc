# Visual Studio Code

## Introduction

Pixel Wrangle propose par défaut la possibilité d'éditer le code de vos shaders au sein de Visual Studio Code.

Visual Studio Code est un éditeur de code très populaire proposé par Microsoft, il est gratuit et dispose d'un écosystème d'extension très large.

Pour plus d'informations, vous pouvez consulter le [site officiel de Visual Studio Code](https://code.visualstudio.com)


## Configuration

Pour profiter de l'intégration de Visual Studio Code au sein de Pixel Wrangle, vous devez préalablement définir le chemin qui pointe vers l'exécutable de Visual Studio Code.

Par défaut il se situe :

- Soit dans votre dossier personnel (si vous effectuez une installation pour votre compte utilisateur uniquement) => C:\\Users\\<**YOUR-USER-ACCOUNT-NAME**>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe
- Soit dans votre partition système (si vous effectuez une installation pour tout les utilisateur ->  requiert les droits Administrateur) => C:\\ProgramData\\Microsoft VS Code\\Code.exe

Pour définir le chemin de Visual Studio Code, vous avez 2 possibilités

- Via l'interface de Pixel Wrangle : (Si le paramètre est bloqué, vous pouvez faire un clic droit puis toggle read-only afin d'activer le mode 'Edition')
![[vs-code-path-from-parameters.png]]

- Via le fichier settings.json situé dans le dossier racine du projet :
![[vs-code-path-from-json.png]]

Une fois cela fait, vous pourrez lancer l'édition de votre code dans l'éditeur Visual Studio Code en pressant le bouton correspondant dans l'UI, ou en pressant **[CTRL] + E** dans l'UI.
Ce chemin sera ensuite commun à toutes vos instances de Pixel Wrangle dans tout vos projets.

## Environnement

Par défaut, au premier lancement de Visual Studio Code via Pixel Wrangle, un nouvel environnement de travail Visual Studio Code est créé.
Il sera utilisé par la suite dès lors que vous ouvrirez Visual Studio Code via une instance de Pixel Wrangle.
L'objectif est que cela n'interfère pas avec vos environnements Visual Studio Code existants ainsi que vous permettre de le personnaliser à votre goût (raccourcis, extensions, etc).
De plus, cet environnement sera conservé au fur et à mesure des updates de Pixel Wrangle car il est stocké localement sur votre machine et reste indépendant de l'installation de Pixel Wrangle
Par défaut, l'emplacement de cet environnement est : C:\\Users\\<**YOUR-USER-ACCOUNT-NAME**>\\.vscode-td\\glsl

## Emplacement des fichiers temporaires de Pixel Wrangle

Lorsque vous écrivez du code dans Pixel Wrangle, à chaque modification, les fichiers sont sauvés sur le disque dur.
L'emplacement de ces fichiers se situe dans le dossier temporaire de Touchdesigner généralement situé dans

## Profil .code-profile fourni

Pour commencer, Pixel Wrangle propose un profil Visual Studio Code qui propose un jeu d'extensions, de raccourcis claviers. Cette configuration est volontairement minimaliste et adaptée pour l'écriture de GLSL. Par ailleurs, quelques variables spécifiques à l'environnement GLSL dans Touchdesigner sont injectées, ce qui permet d'avoir une meilleure auto-complétion sans avoir à réaliser d'imports.

Vous pouvez charger directement ce profil dans Visual Studio Code via la palette de commande ([CTRL] + [SHIFT] + P), et lancer la commande : 'Import Settings Profile'

![[vs-code-import-profile.png|600]]

Si vous souhaitez charger ce fichier de profil dans votre environnement, vous pouvez aller le chercher à la racine du projet Pixel Wrangle dans votre Palette

![[vscode-profile-path.png|600]]