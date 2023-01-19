# Coding with Pixel-Wrangle

## Introduction

Pixel Wrangle offers a pixel shader development environment involving an underlying GLSL TOP. It therefore uses the GLSL language because it inherits from GLSL TOP. GLSL TOP is a GLSL implementation offered by Touchdesigner in its own ecosystem, it fits into the workflow of TOPs (Texture Operators in the Touchdesigner terminology). For more information on the GLSL language, visit the [[GLSL]] page. For more information on TOPs in Touchdesigner, go [here](https://derivative.ca/UserGuide/TOP).

### Overview of the Pixel Wrangle editor

Editing code in Pixel Wrangle is divided into 3 tabs in the [[en/index]]:

- The **IO** tab which contains the 'Inputs' (your 'uniform' variables) and 'Outputs' (the shader's output buffers) panels. These panels have specific syntax which is detailed further -> [[Coding with Pixel Wrangle]]
- The **Functions** tab which houses all the code outside the main() function. You can declare your custom functions, macros, shared variables, etc. there.
- The **Main** tab which corresponds to the body of the main() function. You do not need to declare this function within your code, just write your code as you would in the main() function.

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);" width="100%" height="500" src="https://www.figma.com/embed?embed_host=share&url=https%3A%2F%2Fwww.figma.com%2Fproto%2FDZoDzaIA6ZhcCWlU0Gr8RC%2FPixel-Wrangle-UI%3Fnode-id%3D153%253A165%26scaling%3Dscale-down%26page-id%3D153%253A164%26starting-point-node-id%3D153%253A165" allowfullscreen></iframe>

### Execution order

The code content of the 3 tabs is parsed, then sent to the compiler in the following order:

0. Macros
1. IO -> Inputs (uniform)
2. Functions -> imports with \#include + function declarations
3. IO -> Outputs (Buffers)
4. Main (main() function with your code inside)

To view the complete code (once parsed: that is, as it is sent to the compiler), you can consult the 'Full Code Panel' ([CTRL] + 9)

### **Attention**

- Both the 'Functions' and 'Main' tabs use standard GLSL syntax, so you can write code exactly as you would in any GLSL shader.

#### **BUT**

- **The '[[Coding with Pixel Wrangle#IO tab specifics|IO]]' tab has a special syntax**, explained in detail [[Coding with Pixel Wrangle#IO tab specifics|below]]

## IO tab specifics

### **Inputs panel (uniform variables)**

#### Syntax specifics

- Each of the variables you declare in the Inputs panel are uniform variables mapped to a Touchdesigner parameter.
- **No 'uniform' keyword**, it is implicit and is added automatically when parsing by Pixel Wrangle, before compilation.
- Your **uniform variable name reflects the name of the associated Touchdesigner parameter**.
- Consequently, **your variable name must absolutely start with a capital followed by lowercase letters only, and NO special characters or '\_'** (constraint of Touchdesigner parameter names)
- The type of the variable determines the type of the Touchdesigner parameter that will be associated with it, following the same scheme as your uniforms declarations in a GLSL TOP
- \*\*Any line of code starting with '//' or '/\*' is considered a comment, EXCEPT [[Coding with Pixel Wrangle#Specific cases (Headers)|Specific cases]] (Headers)
- \*\*Any variable declaration or comment must be made on a single line.
- \*\*To customize the Touchdesigner parameter associated with your uniform variable, you can add a comment after the declaration of your variable followed by pairs of property=value detailed [[Coding with Pixel Wrangle#Modifiable properties|here]]
- [[Coding with Pixel Wrangle#Modifiable properties|Modifiable properties]] reflect what you could do via the Component Editor, no more.
- \*\*Each property must be followed by a '=' sign then its value.
- \*\*Each [property=value] pair must be separated by a ';'
- The list of modifiable properties is available below, in the 'Modifiable properties' section
- Properties whose value is a string must not be enclosed in ' or ", but written directly as they are.
- However, properties whose value is an array of string must be written in the form \[ 'Menu Entry 1' , 'Menu Entry 2' ]\ with ' or " enclosing each element of the array.

#### Specific cases (Headers)

- Header type parameters are available although they have no meaning at the GLSL level, as they are useful for formatting the UI.

  - The syntax for creating a Header type parameter is '//--- '.
  - You can then associate a label and/or section property with it.

  For example: '//--- label=Noise Types; section=1' will create an UI parameter of type Header, with the title: 'Noise Types', and will have a separator above it.

- Other parameters not compatible as 'uniform' with GLSL TOP such as 'File' or 'Folder' types are not available through the IO panel.

#### Correspondence of GLSL Types / TD parameter styles

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

#### Modifiable properties

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

#### Examples

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

### **Outputs panel (Buffers)**

#### Generalities within GLSL TOP

Buffers are variables that allow storing and sharing data between different shaders or different rendering stages. They are useful for accumulating data and communicating between the different shaders of a rendering pipeline.

Here are the 2 most used forms within the Touchdesigner GLSL TOP:

- Texture buffers (**Accessible in Vertex/Pixel AND Compute mode**): They allow referencing textures that will be used for display. They are generally used in shaders to **READ** texture pixels and use them to influence the final color of the pixel.

- Image buffers (**Accessible in Compute mode ONLY**): They allow **READING AND WRITING** to image data buffers such as textures or offscreen renderings. They are generally used in shaders to perform image processing operations.

It is also possible to declare image texture buffers with dimensions greater than 2 (for example, a 3D texture or a 3D image).

It is important to note that texture buffers and image buffers cannot be used interchangeably.
Each has its own syntax and its own access functions.

Also, it should be noted that image buffers can be accessible for reading and writing but only in 'Compute Shader' mode, refer to the Pixel Wrangle or GLSL TOP parameters page to adjust this option (Output Access).

Regarding **texture buffers**: they are **accessible in READ ONLY mode but available in both modes** (Vertex/Pixel AND Compute)\*\* with the texture() function.

Furthermore, within a Pixel Shader **in 'Compute' mode, the use of texelFetch() is more appropriate** for reading texture data as it generally offers better performance while allowing the management of mipmap levels (or LOD => Level Of Detail).

#### Examples in a GLSL TOP

##### **Vertex/Pixel Mode**

```glsl
// Declare a buffer in Pixel/Vertex mode
layout(location = 0) out vec4 fragColor;
layout(location = 1) out vec4 otherColor;
layout(location = 2) out vec4 extraInfo;

void main(){

/* Assign a value to a buffer */

// Write the red color to the 'fragColor' buffer
fragColor = vec4(1,0,0,1); // Buffer 0 = Red

// Write the color of the first 2D input, sampled with the normalized vUV.st coordinates provided by Touchdesigner.
otherColor = texture(sTD2DInputs[0], vUV.st); // Buffer 1 = Texture of the first input

// Write the blue color to the 'fragColor' buffer
extraInfo = vec4(0,0,1,1); // Buffer 2 = Blue

}
```

##### **Compute Mode**

```glsl
/*
Compute shaders do not have an opinion on the type of data you provide them with, or on how to distribute calculations across the GPU. These aspects offer great flexibility but require determining in advance how you want calculations to be performed on your graphics card.

For more information on this, you can refer to the official GLSL documentation. For a brief explanation of the concept and its implementation in TD, visit https://docs.derivative.ca/Compute_Shader and https://docs.derivative.ca/Write_a_GLSL_TOP
*/

 // Declare the size of the thread groups
layout (local_size_x = 8, local_size_y = 8) in;

/*
The group size is used to determine how texture data is partitioned and distributed to threads on your GPU.
For example, if you use a 512 x 512 pixel texture and a group size of 8 x 8, you will need 64 thread groups to cover the entire texture.

If you leave the Auto-dispatch parameter on ON in the GLSL page settings, GLSL TOP will automatically assign the number of necessary groups to cover the full resolution chosen, so here: 64. (512/8 for each dimension).
In the other case, you will need to manually set the number of thread groups using the Dispatch Size parameter.
*/


void main(){

 // Define the color
 vec4 color = vec4(1,0,0,1); // Buffer 0 = Rouge

 // Write red color in Buffer 0
 imageStore(mTDComputeOutputs[0], ivec2(gl_GlobalInvocationID.xy), color);

}
```

#### Managing buffers with Pixel Wrangle

- Declare your buffers directly in the 'Outputs' panel of the 'IO' tab, in the same way, whether you are in Vertex/Pixel mode or Compute mode. In general (although there is no absolute rule), I recommend using CAPITAL NAMES to clearly differentiate your output buffers from your Inputs. Below is an example of declaring 3 output buffers

```glsl
vec4 IMAGE;
vec4 POSITION;
vec4 NORMALS;
```

- Declare your buffers directly in the 'Outputs' panel of the 'IO' tab, always in the same way, whether you are in Vertex/Pixel mode or in Compute mode. In general (although there is no absolute rule), I recommend using names in CAPITALS to clearly differentiate your output buffers from your Inputs. Below is an example of declaring 3 output buffers.

- Pixel Wrangle creates the necessary outputs immediately with their respective names, you will see them appear on the OP instance as TOP type outputs.

- At the same time, a tab corresponding to each of your declarations will be visible in the 'Viewer' panels.

- Outputs are mapped in the order in which they were declared. If you want to change this order, you can change the order of your declarations.

- By default, each of these declarations dynamically adds a corresponding macro to easily access them in Compute Shaders. To take the previous example, this is what it looks like in the code sent to the compiler:

```glsl
#define IMAGE mTDComputeOutputs[0]
#define POSITION mTDComputeOutputs[1]
#define NORMALS mTDComputeOutputs[2]
```

- Then, you can write to these buffers as follows from the 'Main' tab:

##### **Vertex/Pixel Mode**

```glsl
/* Read Position-Normalized-Centered-Ratio from macro and store it in a vec4 called 'c' */
// _PNCR is a Shorthand for Position-Normalized-Centered-RatioAlongMaxAxis
vec4 c = vec4(_PNCR,1);

/* Write vec4 'c' in 'IMAGE' Buffer, td() is a shorthand for TDOutputSwizzle() */
IMAGE = td(c)

/*

To learn more about the macros used above, you can go to the [[Macros]] page, where you will find a complete list of macros that can be used in Pixel Wrangle. They can also be viewed from the 'Macros' panel in the upper left of the interface, or directly from the 'Full Code' panel as they are sent to the compiler.

*/
```

##### **Compute Mode**

```glsl
/* Read Position-Normalized-Centered-Ratio from macro and store it in a vec4 called 'c' */
// _PNCR is a Shortcut for Position-Normalized-Centered-RatioAlongMaxAxis
vec4 c = vec4(_PNCR,1);


/* Write vec4 'c' in 'IMAGE' Buffer using Absolute Coordinates */
// Shortcut for imageStore(mTDComputeOutputs[0], ivec2(gl_GlobalInvocationID.xy), TDOutputSwizzle(c))
store(IMAGE, _UV, td(c));
```
