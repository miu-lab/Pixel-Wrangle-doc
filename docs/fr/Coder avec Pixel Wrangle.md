# Coder avec Pixel-Wrangle

## Introduction

Pixel Wrangle propose un environnement de développement de pixel shaders impliquant un [GLSL TOP](https://docs.derivative.ca/GLSL_TOP) sous-jacent.
Il utilise donc le langage GLSL puisqu'il hérite de GLSL TOP.
GLSL TOP est une implémentation de GLSL proposée par Touchdesigner dans son propre écosystème, elle s'intègre dans le workflow des TOP (Texture Operators dans la terminologie Touchdesigner).
Pour plus d'informations sur le langage GLSL, rendez-vous sur la page [[GLSL]]
Pour plus d'informations sur les TOP dans Touchdesigner, rendez-vous [ici](https://derivative.ca/UserGuide/TOP)

### Vue d'ensemble de l'éditeur de Pixel Wrangle

L'édition du code dans Pixel-Wrangle se divise en 3 onglets dans l'[[fr/index]]:

- L'onglet 'IO' qui contient les panneaux 'Inputs' (vos variables 'uniform') et 'Outputs' (les buffers de sorties du shader). Ces panneaux font l'objet d'une syntaxe spécifique dont le détail complet se trouve ci-après -> [[Coder avec Pixel Wrangle#Les particularités de l'onglet IO|Les particularités de l'onglet IO]]
- L'onglet 'Functions' qui accueille tout le code en dehors de la fonction main(). Vous pouvez y déclarer vos fonctions personnalisées, macros, variables partagées, etc.
- L'onglet 'Main' qui correspond au corps de la fonction main(). Vous n'avez donc pas à déclarer cette fonction au sein de votre code mais juste écrire votre code comme vous le feriez dans la fonction main().

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="100%" height="500" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Fproto%2FDZoDzaIA6ZhcCWlU0Gr8RC%2FPixel-Wrangle-UI%3Fnode-id%3D153%253A165%26scaling%3Dscale-down%26page-id%3D153%253A164%26starting-point-node-id%3D153%253A165" allowfullscreen></iframe>

### Ordre d'exécution

Le contenu en code des 3 onglets sont parsés, puis envoyés au compilateur dans l'ordre suivant :

0. Macros
1. IO -> Inputs (uniform)
2. Functions -> imports avec \#include + déclarations de fonctions
3. IO -> Outputs (Buffers)
4. Main (fonction main() avec votre code à l'intérieur)

Pour visualiser le code complet (une fois parsé : donc tel qu'il est envoyé au compilateur), vous pouvez consulter le 'Full Code Panel' ([CTRL] + 9)

### **Attention**

- Les onglets 'Functions' et 'Main' utilisent tous deux la syntaxe GLSL standard donc vous pouvez écrire le code exactement comme vous l'écririez dans n'importe quel shader GLSL.

#### **MAIS**

- **L'onglet '[[Coder avec Pixel Wrangle#Les particularités de l'onglet IO|IO]]' dispose d'une syntaxe particulière**, expliquée en détail [[Coder avec Pixel Wrangle#Les particularités de l'onglet IO|ci-après]]

## Les particularités de l'onglet IO

### **Panneau des Inputs (variables uniform)**

#### Spécificités de syntaxe

- Chacune des variables que vous déclarez dans le panneau Inputs sont des variables uniforms mappées sur un paramètre de Touchdesigner.
- **Pas de mot clé 'uniform'**, il est implicite et est ajouté automatiquement au moment du parsing par Pixel Wrangle, avant la compilation.
- Le nom de votre **variable uniform reflète le nom du paramètre Touchdesigner associé**.
- En conséquence, **le nom de votre variable doit impérativement commencer par une majuscule suivie de minuscules uniquement, et SANS caractères spéciaux ou '\_'** (contrainte des noms de paramètres Touchdesigner)
- Le type de la variable détermine le type du paramètre Touchdesigner qui y sera associé en suivant le même schéma que les déclarations de vos uniforms dans un GLSL TOP
- **Toute ligne de code débutant par '//' ou '/\*' est considérée comme un commentaire, SAUF [[Coder avec Pixel Wrangle#Cas spécifiques (Headers)|Cas spécifiques]] (Headers)
- **Toute déclaration de variable ou commentaire doit se faire sur une seule et unique ligne.
- **Pour personnaliser le paramètre Touchdesigner associé à votre variable uniform, vous pouvez ajouter un commentaire à la suite de la déclaration de votre variable suivi de paires propriété=valeur détaillées [[Coder avec Pixel Wrangle#Les propriétés modifiables|ici]]
- Les [[Coder avec Pixel Wrangle#Les propriétés modifiables|propriétés modifiables]] reflètent ce que vous pourriez faire via le Component Editor, pas plus.
- **Chaque propriété doit être suivie d'un signe '=' puis de sa valeur.
- **Chaque couple [propriété=valeur] doit être séparé par un ';'
- La liste des propriétés modifiables est consultable ci-dessous, dans la rubrique 'Propriétés modifiables'
- Les propriétés dont la valeur est une string ne doivent pas être englobées dans des ' ou des ", mais écrites directement, telles quelles.
- Néanmoins les propriétés dont la valeur est un array de string doivent être écrites sous la forme \[ 'Menu Entry 1' , 'Menu Entry 2' ]\ avec des ' ou des " englobant chaque élément de l'array.

#### Cas spécifiques (Headers)

- Les paramètres de type Header sont disponibles bien qu'ils n'aient pas de sens au niveau de GLSL, car ils sont utiles pour la mise en forme de l'UI.
  - La syntaxe pour créer un paramètre de type Header est '//--- '.
  - Vous pouvez ensuite lui associer une propriété label et/ou section.
  
  Par exemple : '//--- label=Noise Types; section=1' créera un paramètre d'UI de type Header, avec pour titre : 'Noise Types', et disposera d'un séparateur au dessus.

- Les autres paramètres non compatibles en tant que 'uniform' avec GLSL TOP comme les types 'File' ou 'Folder' ne sont pas disponibles via le panneau IO.

#### Correspondance des Types GLSL / Style de paramètres TD

| **GLSL Type**  | **TD Parameter Style**                    | **Syntax**                                                                                                                       |
| -------------- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| int            | Int OR Menu OR Toggle OR Pulse            | int Newint;                                                                                                                      |
| float          | Float                                     | float Newfloat;                                                                                                                  |
| vec[2-4]       | Float of size [2-4] OR Color for vec[3-4] | vec3 Newcolor;                                                                                                                   |
| uvec[2-4]      | Int of size [2-4] OR Color for vec[3-4]   | uvec2 Newunsignedvec;                                                                                                            |
| ivec[2-4]      | Int of size [2-4] OR Color for vec[3-4]   | ivec2 Newsignedvec;                                                                                                              |
| bvec[2-4]      | Int of size [2-4] OR Color for vec[3-4]   | bvec2 Newboolvec;                                                                                                                |
| \<TYPE\>\[\*\] | CHOP                                      | vec4 Newvectorarray[16] => !! Array of 16 vec4 values, needs to be referenced by a CHOP that holds the corresponding channels !! |
| mat4           | CHOP                                      | mat4 Newmatrix;                                                                                                                  |
| atomic_uint    | Int                                       | atomic_uint Newatomicint;                                                                                                        |
| const int      | Int OR Menu OR Toggle OR Pulse            | const int Newconstantint = 0;                                                                                                    |

#### Les propriétés modifiables

| **Syntax** | **Description**                            | **Default Value**                                                                                                                                                        |
| ---------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| label      | Parameter Label                            | Parameter Name                                                                                                                                                           |
| section    | Parameter has separator                    | 0                                                                                                                                                                        |
| order      | Parameter UI order (position)              | integer => Same as your declaration order                                                                                                                                |
| readonly   | Parameter Lock                             | 0                                                                                                                                                                        |
| enable     | Parameter Activation                       | 1                                                                                                                                                                        |
| enableExpr | Parameter Activation Expression            | ''                                                                                                                                                                       |
| help       | Parameter Help text on hover               | ''                                                                                                                                                                       |
| style      | Parameter Style                            | Float or Color (for vec3-4) or Str or CHOP (according to variable type). Can be modified for int, float, vec3-4 (Valid values => Menu, Color, Toggle, Pulse, Float, Int) |
| expr       | Parameter Expression                       | ''                                                                                                                                                                       |
| bindExpr   | Parameter Bind Expression                  | ''                                                                                                                                                                       |
| menu       | Parameter Menu entries (integer type only) | ''                                                                                                                                                                       |
| default    | Parameter Default Value                    | 0 or ''                                                                                                                                                                  |
| min        | Minimum Parameter Value                    | 0                                                                                                                                                                        |
| max        | Maximum Parameter Value                    | 1                                                                                                                                                                        |
| toggle     | Parameter is a toggle (integer type only)  | ''                                                                                                                                                                       |
| pulse      | Parameter is a Pulse (integer type only)   | ''                                                                                                                                                                       |
| chop       | Parameter chop path (mat4 type only)       | ''                                                                                                                                                                       |

#### Exemples

```glsl
/* Syntax example */

// Single line comments are ignored by the parser
// Except for header (when line start with //---)


//--- label=Simple Parameters; section=1

// Create Float Parameter
float Float; // default=0; min=-1; max=1; section=1

// Create Integer Parameter
int Int;     // min=-10; max=10; default=0


//--- label=Vector Parameters; section=1

// Create Color Vector Parameter
vec3 Color;  // default=[0, 0.7, 1]; section=1

// Create Vector style Parameter
vec3 Position;  // default=[0, 0.7, 1]; style=Float


//--- label=Menu Parameters; section=1

// Create Menu Parameter
int Newmenu;                  // menu=['Tomatoes', 'Bananas']; default=1; section=1


//--- label=Array Parameters; section=1

//// Create a uniform array of vec2 (you will need to specify a CHOP target with 2 channels and 16 samples each)
// vec2 Array[16];  

//// Create a texture buffer array of vec4 (you will need to specify a CHOP target with 4 channels and 200 samples each)
// vec4 Array[200]; 

//// Create Matrix Parameter (you will need to specify a compatible 'chop' like transformCHOP to remove warning)
// mat4 NewMatrix;


/* Vulkan Specialization Constants */
const int ConstanttypeMenu = 0;     // menu=['Candies', 'Sky']; label=Vulkan Constant Menu


//--- label=Atomic Counters; section=1

// Create Atomic Counter
atomic_uint NewAtomicCounter; // section=1
```

### **Panneau des Outputs (Buffers)**

#### Généralités au sein de GLSL TOP

Les buffers sont des variables qui permettent de stocker et de partager des données entre différents shaders ou différentes étapes de rendu. Ils sont utiles pour l'accumulation de données et la communication entre les différents shaders d'un pipeline de rendu.

Voici les 2 formes les plus utilisées au sein du GLSL TOP de Touchdesigner :

- Les buffers de textures (**Accessible en mode Vertex/Pixel ET Compute**): Ils permettent de référencer des textures qui seront utilisées pour l'affichage. Ils sont généralement utilisés dans les shaders pour **LIRE** des pixels de texture et les utiliser pour influencer la couleur finale du pixel.

- Les buffer d'images (**Accessible en mode Compute UNIQUEMENT**) : Ils permettent de **LIRE ET ÉCRIRE** dans des tampons de données d'images tels que des textures ou des rendus offscreen. Ils sont généralement utilisés dans les shaders pour effectuer des opérations de traitement de l'image.

Il est également possible de déclarer des buffers de textures d'images avec des dimensions supérieures à 2 (par exemple, une texture 3D ou une image 3D).

Il est important de noter que les buffers de texture et d'image ne peuvent pas être utilisés de manière interchangeable.
Chacun a sa propre syntaxe et ses propres fonctions d'accès.

Également, il est à noter que les buffers d'image peuvent être accessibles en écriture et en lecture mais seulement en mode 'Compute Shader', se référer à la page GLSL des paramètres de Pixel Wrangle ou d'un GLSL TOP afin de régler cette option (Output Access).

Concernant **les buffers de texture** : ils **sont accessibles en lecture UNIQUEMENT mais disponible dans les 2 modes** (Vertex/Pixel ET Compute)** avec la fonction texture().

Par ailleurs, au sein d'un Pixel Shader **en mode 'Compute', l'utilisation de texelFetch() est plus appropriée** pour lire les données de texture car elle offre globalement de meilleures performances tout en permettant la gestion des niveaux de mipmap(ou LOD => Level Of Detail).

#### Exemples dans un GLSL TOP

**En mode Vertex/Pixel :**

```glsl
 // Déclarer un buffer en mode Pixel/Vertex 
 layout(location = 0) out vec4 fragColor;
 layout(location = 1) out vec4 otherColor;
 layout(location = 2) out vec4 extraInfo;

void main(){
 
 /* Assigner une valeur à un buffer */
 
 // Ecrire la couleur rouge dans le buffer 'fragColor'
 fragColor = vec4(1,0,0,1); // Buffer 0 = Rouge
 
 // Ecrire la couleur de la première entrée 2D, échantillonnée avec les coordonnées normalisées vUV.st fournies par Touchdesigner.
 otherColor = texture(sTD2DInputs[0], vUV.st); // Buffer 1 = Texture de la première entrée
 
 // Ecrire la couleur bleu dans le buffer 'fragColor'
 extraInfo = vec4(0,0,1,1); // Buffer 2 = Bleu
 
}
```

**En mode Compute:**

```glsl
/* 
Les Compute Shaders n'ont pas d'opinion sur le type de données que vous leur fournissez, ni sur la manière de répartir le calcul sur le GPU. 
Ces aspects offrent une grande flexibilité mais nécessitent de déterminer en amont la façon dont vous souhaitez que les calculs s'effectuent sur votre carte graphique. 

Pour plus d'informations à ce sujet, vous pouvez vous référer à la documentation officielle de GLSL.
Pour une explication brève du concept et son implémentation au sein de TD, rendez-vous sur https://docs.derivative.ca/Compute_Shader et https://docs.derivative.ca/Write_a_GLSL_TOP
*/

 // Déclarer la taille des groupes de thread
layout (local_size_x = 8, local_size_y = 8) in;

/*
La taille de groupe est utilisée pour déterminer comment les données de texture sont partitionnées et distribuées aux threads de votre GPU. 
Par exemple, si vous utilisez une texture de 512 x 512 pixels et une taille de groupe de 8 x 8, vous aurez besoin de 64 groupes de threads pour couvrir la totalité de la texture.

Si vous laissez le paramètre Auto-dispatch sur ON dans les paramètres de la page GLSL, GLSL TOP assignera automatiquement le nombre de groupe nécessaires afin de couvrir l'intégralité de la résolution choisie, donc ici : 64. (512/8 pour chaque dimension).
Dans le cas contraire, vous devrez définir manuellement le nombre de groupe de threads via le paramètre Dispatch Size.
*/


void main(){

 // Définir la couleur 
 vec4 color = vec4(1,0,0,1); // Buffer 0 = Rouge
 
 // Ecrire la couleur rouge dans le buffer 0
 imageStore(mTDComputeOutputs[0], ivec2(gl_GlobalInvocationID.xy), color);
 
}
```

#### Gestion des buffers avec Pixel Wrangle

- Déclarez vos buffers directement dans le panneau 'Outputs' de l'onglet 'IO', toujours de la même manière, que vous soyez en mode **Vertex/Pixel** ou en mode **Compute**. De manière générale (bien qu'il n'y aie pas de règle absolue), je vous conseille d'utiliser des noms en CAPITALES afin de bien différencier vos buffers de sortie de vos Inputs. Ci-dessous un exemple de déclaration de 3 buffers de sortie

```glsl
vec4 IMAGE;
vec4 POSITION;
vec4 NORMALS;
```

- Pixel Wrangle crée immédiatement les sorties nécessaires avec leurs noms respectifs, vous les verrez apparaître sur l'instance de l'OP en tant que sorties de type TOP.

- Dans le même temps, un onglet correspondant à chacune de vos déclarations sera visible dans les panneaux 'Viewer'

- Les sorties sont mappées dans l'ordre où elles ont été déclarées. Si vous souhaitez modifier cet ordre, vous pouvez changer l'ordre de vos déclarations.

- Par défaut, chacune de ces déclarations ajoute dynamiquement une macro correspondante pour y accéder facilement dans les Compute Shaders. Pour reprendre l'exemple précédent, voici ce que cela donne dans le code envoyé au compilateur :

```glsl
#define IMAGE mTDComputeOutputs[0]
#define POSITION mTDComputeOutputs[1]
#define NORMALS mTDComputeOutputs[2]
```

- Ensuite, vous pouvez donc écrire dans ces buffers de la manière suivante depuis l'onglet 'Main' :

##### **En mode Vertex/Pixel**

```glsl
/* Read Position-Normalized-Centered-Ratio from macro and store it in a vec4 called 'c' */
// _PNCR is a Shorthand for Position-Normalized-Centered-RatioAlongMaxAxis
vec4 c = vec4(_PNCR,1); 

/* Write vec4 'c' in 'IMAGE' Buffer, td() is a shorthand for TDOutputSwizzle() */
IMAGE = td(c)

/* 

Pour en savoir plus sur les macros utilisées ci-dessus, vous pouvez vous rendre sur la page [[Macros]], vous y trouverez une liste complète des macros utilisables dans Pixel Wrangle.
Elles sont également consultables depuis le panneau 'Macros' en haut à gauche dans l'interface, ou directement depuis le panneau 'Full Code' telles qu'elles sont envoyées au compilateur.

*/
```

##### **En mode Compute**

```glsl
/* Read Position-Normalized-Centered-Ratio from macro and store it in a vec4 called 'c' */
// _PNCR is a Shortcut for Position-Normalized-Centered-RatioAlongMaxAxis
vec4 c = vec4(_PNCR,1); 


/* Write vec4 'c' in 'IMAGE' Buffer using Absolute Coordinates */
// Shortcut for imageStore(mTDComputeOutputs[0], ivec2(gl_GlobalInvocationID.xy), TDOutputSwizzle(c))
store(IMAGE, _UV, td(c));
```
