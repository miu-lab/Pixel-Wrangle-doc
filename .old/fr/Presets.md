# Presets

 

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="100%" height="500px" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Ffile%2FDZoDzaIA6ZhcCWlU0Gr8RC%2FPixel-Wrangle-UI%3Fnode-id%3D153%253A10%26t%3DuJ7kUBcROLYzKe7F-1" allowfullscreen></iframe>

## Généralités

Pixel Wrangle dispose d'un système de gestion de préréglages.
Ces préréglages sont appelés 'preset'

Un preset Pixel-Wrangle contient à la fois tout le code que vous avez écrit dans chacun des panneaux du shader, ainsi que tous les paramètres et leur état au moment de la sauvegarde.

Afin de faciliter le chargement et le filtrage des presets, Pixel Wrangle fournit un explorateur basique avec une hiérarchie fichiers/dossiers qui reflète la structure de votre système de fichier

L'explorateur vous permet de trouver, charger ou sauver des préréglages.

Il est activable en cliquant sur la zone de recherche située en bas à gauche de l'interface ou via le raccourci [CTRL] + Tab.

Vous pouvez ainsi taper le nom de votre recherche afin de filtrer les résultats ou dérouler l'architecture.

## Chemins importants

Les presets sont stockés par défaut dans le dossier Presets à la racine de Pixel Wrangle dans votre Palette.  

Il y a 2 dossiers principaux : 
- Builtins : C'est le dossier des presets livrés avec Pixel Wrangle
- User : C'est le dossier où sont sauvés par défaut vos presets

NB : Si vous souhaitez construire votre propre architecture dans votre dossier User, vous pouvez le faire depuis l'explorateur de fichier de votre système via un clic sur l'icone de dossier en bas à gauche de l'interface ou le raccourci [CTRL] + O.  
Tout les sous-dossiers créés sont reflétés dans l'explorateur de Pixel Wrangle.

## Charger un preset

Pour charger un preset, il vous suffit de double cliquer dessus depuis l'explorateur de Pixel Wrangle.

**Attention, parfois le chargement peut être erratique, vous devrez peut-être effectuer un double-clic à plusieurs reprises.
Ceci est un bug connu qui devrait être fixé sur les prochaines version de Pixel Wrangle**

## Sauver un preset

Pour sauver l'état de votre shader dans un nouveau preset, vous pouvez cliquer sur '+' dans l'UI. Cela vous ouvrira un pop-up qui vous demandera sous quel nom vous souhaitez sauvegarder ce préréglage. Par défaut, ce preset sera sauvé dans le dossier Presets/User depuis la racine de Pixel Wrangle dans votre Palette
