# Le langage GLSL

 

## Généralités

GLSL est un langage compilé de type C, conçu pour créer des shaders.
Il a été créé et est maintenu par le consortium à but non-lucratif Khronos Group qui est à l'origine du standard open-source OpenGL (et maintenant Vulkan qui à terme prendra sa place).

Un shader est un petit programme informatique réduit à un champ d'action spécifique.
Il est généralement destiné à afficher des choses à l'écran.
Les shaders en GLSL sont exécutés sur votre processeur graphique (GPU).

GLSL est en mesure d'intervenir à plusieurs niveau du processus de rendu d'une image.

Toutes les étapes nécessaires pour fournir le résultat final est appelé le [[GLSL#Pipeline de rendu|pipeline de rendu]].

GLSL s'intègre donc au sein d'applications graphiques utilisant les API OpenGL, Vulkan, WebGL, et plus encore...

Depuis les builds 2022, Touchdesigner intègre GLSL au travers de Vulkan, la dernière API graphique de Khronos qui finira par remplacer OpenGL à terme.

Le logiciel propose une implémentation de GLSL via 2 de ses opérateurs natifs : [GLSL TOP](https://docs.derivative.ca/GLSL_TOP) (Texture) et [GLSL MAT](https://docs.derivative.ca/GLSL_MAT) (Materials)

Il est important de noter que les shaders en GLSL s'exécutent de manière parallèle sur votre machine.
Ils permettent donc de tirer parti de la puissance des processeurs graphiques (GPU) et offrent de très bonnes performances pour des applications nécessitant du temps réel.

Quelques fondamentaux en programmation sont toutefois nécessaires pour exploiter pleinement le potentiel des shaders ou les adapter à ses propres besoins.
De plus, la programmation d'un GPU (parallélisme des calculs) implique une structure du code particulière et certaines contraintes qui peuvent mettre un peu de temps à être appréhendées.

Je ne peux que vous conseiller la lecture de l'excellent <https://thebookofshaders.com> qui permet de mieux comprendre ces concepts, d'autant qu'une grande partie du contenu y est traduit dans de nombreuses langues.

Enfin, GLSL découlant directement du langage C, la syntaxe et la sémantique en sont très proche.

Cependant, quelques différences notables par rapport au C sont présentes, notamment :

- Une syntaxe raccourcie pour manipuler des vecteurs (appelée vector swizzling)
- Une bibliothèque native comportant des fonctions mathématiques usuelles ainsi que d'autres fonctions spécifiques aux shaders
- Des types spéciaux (matrices, samplers, etc.) dédiés à la manipulation des shaders.
- Ainsi que plein d'autres choses que je vous invite à découvrir par vous même, via la [documentation officielle](https://registry.khronos.org/OpenGL/specs/gl/GLSLangSpec.4.60.pdf).

Vous découvrirez également que certaines fonctionnalités de C sont absentes.

Par exemple certains types comme les chaînes de caractères n'ont pas d'existence au sein de GLSL.
Ceci est en partie dû aux contraintes qu'impose l'architecture des GPU actuels, ainsi que sur leur manière d'effectuer des calculs.

Pour finir, vous trouverez ci-dessous une liste de liens utiles dans votre parcours.

## Liens utiles

> [!INFO]
>
> - Une [ressource indispensable pour bien commencer si vous n'avez jamais manipulé de shaders (The Book of Shaders)](https://thebookofshaders.com).
>
> - Une [vue générale des spécifications du langage en vidéo.](https://www.youtube.com/watch?v=uOErsQljpHs)
>
> - La [documentation officielle du langage](https://registry.khronos.org/OpenGL/specs/gl/GLSLangSpec.4.60.pdf ) accessible sur le site de Khronos via un PDF à télécharger.
>
> - Un index des [fonctions inclues dans la bibliothèque standard de GLSL](https://docs.gl/sl4/sin).
>
> - Plus de détails sur l'[implémentation de GLSL TOP](https://docs.derivative.ca/Write_a_GLSL_TOP) (l'opérateur sous-jacent de Pixel Wrangle) dans Touchdesigner.
>
> - [Shadertoy](https://shadertoy.com), un site web qui permet la création de shaders en ligne via WebGL. C'est un endroit fantastique où vous trouverez de nombreux exemples de shaders créés par la communauté.
>
> - [Le site web de Inigo Quiles](https://iquilezles.org/articles/) est également une source d'apprentissage massive pour comprendre différents mécanisme liés à la création de shaders et aux maths sous-jacentes.
>
> - [Lygia](https://lygia.xyz) est un projet communautaire porté entre autres par Patricio Gonzalez Vivo (The Book of Shaders), il s'agit d'une bibliothèque de fonctions GLSL (et HLSL) qui délivre une quantité importante de fonctions très utiles. Elle est nativement fournie avec Pixel Wrangle et facilite grandement le travail.
>
> - De nombreuses vidéos YouTube traitent de la création de shaders. Certaines dans le contexte de Touchdesigner, Unity, Shadron, glslsandbox, ou d'autres frameworks. La chaîne YouTube The Art Of Code propose par exemple une bonne introduction par la pratique à GLSL sur Shadertoy.

## Pipeline de rendu

GLSL est un langage pouvant être utilisés à différentes étapes du pipeline de rendu.

Chaque étape du pipeline dispose de certaines variables et fonctions qui lui sont propre

Voici une liste (non exhaustive et probablement inexacte sur certains points) des étapes du pipeline, dans un ordre chronologique :

- Vertex Specification : Il s'agit de l'étape initiale, elle consiste à récupérer la liste des points et primitives ainsi que leurs attributs (id, position, etc...) dans la mémoire du CPU puis les convertir dans un format exploitable pour la suite des étapes du pipeline de rendu dans la mémoire du GPU. Cette étape est réalisée de manière automatique par Touchdesigner et le backend OpenGL/Vulkan lors de l'utilisation d'un [GLSL TOP](https://docs.derivative.ca/GLSL_MAT) sur un [Geometry COMP](https://docs.derivative.ca/Geometry_COMP)

- Vertex Shader : C'est un programme qui s'applique au niveau de la géométrie d'un objet 3D, il est exécuté simultanément sur chaque vertex de la géométrie d'entrée. Il permet notamment de créer et modifier des attributs comme la position des points pour déformer un objet par exemple. Le résultat des ces opérations est ensuite passé à l'étape suivante du pipeline de rendu. Certaines étapes intermédiaires peuvent être effectuées à ce stade (se référer aux Geometry Shaders, Tessellation Shaders...)

- Rasterization : La rasterization est une opération qui permet de transformer une scène décrite dans un espace en 3 dimensions, en une image en 2 dimensions affichable par un écran à une résolution choisie (discrétisation). Vous pouvez imaginer cette étape comme la projection de notre espace écran (la vue), sur le ou les objets 3D de la scène. Cette opération est réalisée par le backend OpenGL / Vulkan de manière transparente et automatique. Il n'y a pas de shader associé permettant d'avoir un contrôle sur cette étape du pipeline mais elle est cruciale dans le pipeline de rendu

- Pixel Shader (ou Fragment Shader) : C'est un programme qui s'exécute sur chaque pixel rendu à l'écran. Un pixel shader permet de manipuler ces différentes données afin de produire la couleur finale des pixels.

 -> Dans le cas d'un objet 3D issu d'un vertex shader, le Pixel shader effectue une interpolation des valeurs des attributs fournis par le vertex shader. Vous pouvez ensuite les manipuler à votre guise pour obtenir le résultat souhaité

 -> Dans le cas où il n'est pas fourni de Vertex Shader. L'implémentation du Pixel shader de Touchdesigner (GLSL TOP) fourni de base 2 triangles rectangle qui forment un rectangle aux dimensions exactes de l'écran. Vous disposez donc d'un système de coordonnées à partir du quel vous pouvez vous amuser. A noter qu'il est tout à fait possible de créer des images 3D sans passer par le vertex shader ni une géométrie composée de points. Cela implique que toute la logique soit codée dans le pixel shader. Le Raymarching (ou 'Sphere Tracing') est une de ces techniques. Elle est basée sur la description des formes par des champs de distance appelés SDF. Vous pourrez trouver énormément d'exemples sur [Shadertoy](https://shadertoy.com) et le [le site d'Inigo Quiles](https://iquilezles.org/articles/) si vous souhaitez en savoir plus sur ces techniques. Concernant le ray-marching dans Touchdesigner le projet rayTK porté par tekt est un exemple merveilleux du portage de cette technique dans l'environnement de Touchdesigner. Il simplifie grandement son utilisation au travers d'un écosystème d'opérateurs préconstruits. Un conseil, allez voir ca !

Dans Touchdesigner, GLSL peut être utilisé via [GLSL TOP](https://docs.derivative.ca/GLSL_TOP) (Génération d'images dans le contexte TOP) ou [GLSL MAT](https://docs.derivative.ca/GLSL_MAT) (Génération de matériaux pour des objets 3D).

NB : L'environnement de travail proposé par Pixel Wrangle ne fonctionne actuellement qu'avec [GLSL TOP](https://docs.derivative.ca/GLSL_TOP)

## Pixel Shader

Dans le cas d'un pixel shader sans vertex shader associé manuellement (le comportement par défaut de Pixel Wrangle),  la chose que vous manipulez effectivement au sein de la fonction main() de votre shader est le pixel courant.

Ce pixel possède une caractéristique intrinsèque et fondamentale, sa position dans l'espace écran.
Dans le cas d'une image en 2 dimensions par exemple, il s'agit des coordonnées en x et en y.

Il est important de comprendre que ce même programme est exécuté simultanément sur chacun des pixels composant votre image finale.

Pour une image de 1024 \* 1024 pixels par exemple, ce programme sera exécuté 1024 \* 1024 fois, soit un total de 1 048 576 fois. C'est beaucoup.

Dans le cas d'un processeur standard (CPU), toutes ces exécutions seraient effectuées de manière séquentielle (ou au mieux, réparties sur les quelques cœurs que compte votre CPU).

Un CPU est très efficace pour la plupart des tâches quotidiennes avec un ordinateur et dispose d'un stockage mémoire rapide et conséquent (RAM), en revanche il se révèle peu efficace lorsque vous avez besoin d'effectuer un même traitement sur un grand nombre d'élément à la fois comme c'est le cas pour une image composée de pixels.

Au contraire, un shader s'exécute sur votre processeur graphique (GPU). Les GPU ont une architecture différente des CPU, qui est conçue pour exécuter un grand nombre de tâches à la fois.
A titre de comparaison, un GPU compte de quelques milliers de cœurs à plus de 10000 pour des GPU haut de gamme.
Cela permet de réduire grandement le nombre d'opérations effectuées en un même instant puisque les calculs sont répartis dans chacun des milliers de cœurs que compte votre GPU.

Les principales limitations du GPU :

- Un espace mémoire limité (la VRAM est chère et de taille fixe), cela induit une certaine limite de taille et de complexité des opérations, plus facilement atteignable qu'avec un CPU.
- Une vitesse d'exécution séquentielle (par cœur de GPU) moins élevé que votre CPU
- Les GPU actuels effectuent des calculs avec des nombres exclusivement, les rendant difficilement utilisables pour des tâches manipulant des chaînes de caractère ou certains types de données complexes qui sont courant dans la programmation traditionnelle.
- Les possibilités d'abstraction sont réduites, le périmètre des opérations est limité

### GPU vs CPU par l'exemple

**CPU** :

- Ecrire 2 boucles 'for' imbriquées, itérant chacune sur les coordonnées x et y de l'image.
- Exécuter votre fonction main() avec tout votre code au sein de vos 2 boucles, sur chacun des pixels composant votre image.

```glsl

int ResolutionX = 1024;
int ResolutionY = 1024;

for(int x=0; x<ResolutionX; x++){
 
 for(int y=0; y<ResolutionY; y++){
  
  /* Intrinsic Attributes */
  
  uvec2 absolutePositionOfCurrentPixel   = uvec2(x,y);                                                           // -> Absolute Coordinates   (with x and y integer values between 0 and Resolution-1)
   vec2 normalizedPositionOfCurrentPixel =  vec2(float(x)/float(ResolutionX-1),float(y)/float(ResolutionY-1));   // -> Normalized Coordinates (with x and y float values between 0 and 1) => vUV.st
  
  
  /* Your main() function in a 2D Pixel Shader is executed here */
  
  // Your awesome stuff...
  // ...
 }
}
```

**Pixel Shader (GPU) :**

- Les coordonnées vous sont déjà fournis par le contexte principal par une variable. Nommée vUV dans Touchdesigner
- Ecrivez juste votre code dans la fonction main() en récupérant directement les coordonnées fournies par le shader (vUV)
Cela aurait la forme suivante pour une image de 1024 * 1024 pixels :
Au sein de la fonction main() d'un pixel shader, vous pouvez considérer que chacune de vos instructions sont exécutées simultanément, et que le contexte d'exécution est la partie interne de cette double boucle 'for'.

GLSL et Vulkan se chargent de répartir intelligemment l'exécution du programme simultanément sur tous les cœurs disponibles de votre GPU.

### Coordonnées du pixel

Dans Touchdesigner, les coordonnées x et y normalisées vous sont fournies sous la forme d'un vecteur dans votre shader via une variable fournie directement par le contexte d'exécution.

Il s'agit de la variable [vUV](https://derivative.ca/UserGuide/Write_a_GLSL_TOP#Pixel_Shaders)

```glsl
vec2 coords = vUV.xy; // Will store a copy of current XY coord in 'coords' vec2 variable
```
