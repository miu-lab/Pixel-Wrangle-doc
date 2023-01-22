# Les Macros

 

## Généralités

En GLSL, les macros sont des instructions qui permettent de définir du code réutilisable. Elles sont similaires aux macros de C et sont définies à l'aide de la directive `#define`.
Les macros peuvent être utiles pour simplifier le code et le rendre plus lisible, mais il faut être vigilant à ne pas en abuser car elles peuvent rendre le code difficile à comprendre et à déboguer.
Par définition, l'évaluation d'une macro a lieu au moment de la compilation (static), et non du runtime.
Cela implique qu'elle ne peut pas être modifiée au cours de l'exécution du programme, contrairement aux variables uniform par exemple.
Les cas d'usage sont donc différents.

Voici un exemple de définition de macro simple :

```glsl
#define PI 3.14159265
```

Dans ce cas, chaque occurrence de `PI` dans le code sera remplacée par la valeur `3.14159265` lors de la compilation.

On peut également définir des macros avec des expressions plus complexes, qui agissent comme des fonctions avec des arguments par exemple :

```glsl
#define MIN(a, b) ((a) < (b) ? (a) : (b))
```

Dans ce cas, lorsque la macro `MIN` est utilisée dans le code avec des arguments, elle est remplacée par l'expression conditionnelle qui compare les deux arguments et renvoie le plus petit des deux. Par exemple, l'instruction `MIN(x, y)` sera remplacée par `((x) < (y) ? (x) : (y))`.

## Fichiers importants

Vous trouverez dans le dossier /Macros du repo, les fichiers contenant toutes les macros pré déclarées par Pixel Wrangle dans vos shader.
Libre à vous de rajouter les vôtres.
La table suivante liste toutes les macros que Pixel Wrangle met à votre disposition.

## Macros de variables accessibles dans Pixel Wrangle

| **Nom de la macro** | **Valeur de la macro** | **Commentaire** |
|-----------------|--------------------|--------------|
| \_PI | 3.1415926535897932384626433832795 | PI Value (3.14...) |
| \_TAU | \_PI * 2 | 2 * PI Value |
| \_GOLD | 1.618033988749894 | Golden Ratio |
| \_E            | 2.7182818284590452353602874713527 | Euler Number |
| \_RES           | vec2(uTDOutputInfo.res.z,uTDOutputInfo.res.w) | 2D Resolution in Pixels |
| \_DEPTH          | uTDOutputInfo.depth.y | Depth |
| \_RATIO2D        | vec2(max(\_RES.x/\_RES.y, 1),max(\_RES.y/\_RES.x, 1)) | Ratio from Max Axis |
| \_CENTER         | 0.5 | Center |
| \_POS2DN         | vec2(\_POS2D) / \_RES | 2D Normalized Position (vUV.st) |
| \_POS2DC         | \_POS2D - vec2(\_RES/2) | 2D Centered Position in Pixels |
| \_POS2DNC        | (vec2(\_POS2D) / \_RES - \_CENTER) | 2D Normalized + Centered Position |
| \_POS2DNR        | \_POS2DN * \_RATIO2D | 2D Normalized + Ratio Position |
| \_POS2DNCR       | (vec2(\_RATIO2D) * vec2(\_POS2DN - \_CENTER)) | 2D Normalized + Centered + Ratio Position |
| \_POS3DN         | vec3(\_POS3D) / vec3(\_RES.x,\_RES.y,\_DEPTH) | 3D Normalized Position (vUV.stp) |
| \_POS3DC         | \_POS3D - vec3(vec2(\_RES/2), \_DEPTH/2)                             | 3D Centered Position in Pixels          |
| \_POS3DNC        | \_POS3DN - \_CENTER                                                 | 3D Normalized + Centered Position       |
| \_POS3DNR        | \_POS3DN * vec3(\_RATIO2D, 1/\_DEPTH)                                | 3D Normalized + Ratio Position          |
| \_POS3DNCR       | ( vec3(\_RATIO2D, 1/\_DEPTH) * (\_POS3DN - \_CENTER))                | 3D Normalized + Centered + Ratio Position |
| \_UV             | \_POS2D                                                            | 2D Position in Pixels                     |
| \_UVN            | \_POS2DN                                                           | 2D Normalized Position (vUV.st)            |
| \_UVC            | \_POS2DC                                                           | 2D Centered Position in Pixels             |
| \_UVNC           | \_POS2DNC                                                          | 2D Normalized + Centered Position          |
| \_UVNR           | \_POS2DNR                                                          | 2D Normalized + Ratio Position             |
| \_UVNCR          | \_POS2DNCR                                                         | 2D Normalized + Centered + Ratio Position  |
| \_P              | \_POS3D                                                            | 3D Position in Pixels                     |
| \_PN             | \_POS3DN                                                           | 3D Normalized Position (vUV.stp)            |
| \_PC             | \_POS3DC                                                           | 3D Centered Position in Pixels             |
| \_PNC            | \_POS3DNC                                                          | 3D Normalized + Centered Position          |
| \_PNR            | \_POS3DNR                                                          | 3D Normalized + Ratio Position             |
| \_PNCR           | \_POS3DNCR                                                         | 3D Normalized + Centered + Ratio Position  |
| iChannel[0-3]   | sTD2DInputs[0-3]                                             | 2D Input Channel 0-3 (Alias for ShaderToy inputs) example : iChannel0 to access first 2D Input |
| o[0-7]          | mTDComputeOutputs[0-7]                                     | Output Buffer 0-7 example : (o0 to access first buffer)|
| i[0-7]\_2D       | sTD2DInputs[0-7]                                             | 2D Input Channel 0-7 example : (i1_2D to access second 2D input)|
| i[0-7]\_3D       | sTD3DInputs[0-7]                                             | 3D Input Channel 0-7 example : (i1_3D to access second 3D input)|
| i[0-7]\_Array    | sTD2DArrayInputs[0-7]                                             | 2D Array Input Channel 0-7 example : (i3_Array to access fourth 2D Array input)|
| i[0-7]\_Cube     | sTDCubeInputs[0-7]                                             | Cube Input Channel 0-7 example : (i3_Cube to access fourth Cube input)|
| i[0-7]          | sTD2DInputs[0-7]                                             | 2D Input Channel 0-7 example : (i1 to access second 2D input)|
| fragCoord       | \_UV                | 2D Position in Pixels |

## Macros de fonctions accessibles dans Pixel Wrangle

| **Nom de la macro** | **Valeur de la macro** | **Commentaire** |
|-----------------|--------------------|--------------|
| fetch           | texelFetch(IN, POS, LOD) | fetch(sampler\*D IN, ivec\* POS, int LOD OR int SAMPLE) => vec* |
| tx              | texture(IN, POS, BIAS) | tx(sampler\*D IN, vec\* POS, float OR vec\* OR float[] bias) => float OR vec4 |
| store           | imageStore(OUT, POS, VALUE) | store(gimage\*D OUT, vec\* POS, vec* VALUE) => void (image) |
| td              | TDOutputSwizzle(COLOR) | td(vec4 COLOR) => vec4 |
