# GLSL Language

## Generalities

GLSL is a compiled C-type language designed to create shaders. It was created and is maintained by the non-profit consortium Khronos Group, which is behind the open-source OpenGL standard (and now Vulkan, which will eventually replace OpenGL).

A shader is a small computer program limited to a specific task. It is generally used to display things on the screen. GLSL shaders are run on your graphics processor (GPU).

GLSL is able to intervene at several levels of the image rendering process. All the steps necessary to provide the final result are called the [[GLSL Language#Rendering Pipeline]]. Therefore, GLSL integrates into graphics applications using the OpenGL, Vulkan, WebGL APIs, and more.

As of the 2022 builds, Touchdesigner integrates GLSL through Vulkan, the latest graphics API from Khronos, which will eventually replace OpenGL.

The software provides an implementation of GLSL through two of its native operators: [GLSL TOP](https://docs.derivative.ca/GLSL_TOP) (Texture) and [GLSL MAT](https://docs.derivative.ca/GLSL_MAT) (Materials).

It is important to note that GLSL shaders run in parallel on your machine, taking advantage of the power of graphics processors (GPUs) and offering very good performance for real-time applications.

However, some programming basics are necessary to fully exploit the potential of shaders or adapt them to your own needs. In addition, programming a GPU (parallel calculation) requires a particular code structure and some constraints that may take some time to be understood.

I highly recommend reading the excellent <https://thebookofshaders.com>, which helps to better understand these concepts, especially since much of the content is translated into many languages.

Finally, GLSL is directly derived from the C language, so the syntax and semantics are very similar.

However, there are some notable differences compared to C, including:

- A shortened syntax for manipulating vectors (called vector swizzling)
- A native library containing common mathematical functions as well as other functions specific to shaders
- Special types (matrices, samplers, etc.) dedicated to shader manipulation
- And many other things that I invite you to discover for yourself through the [official documentation](https://registry.khronos.org/OpenGL/specs/gl/GLSLangSpec.4.60.pdf).

You will also discover that some features of C are absent.

For example, certain types such as strings do not exist in GLSL. This is partly due to the constraints imposed by the architecture of current GPUs and the way they perform calculations.

Finally, you will find a list of useful links below for your journey.

## Useful Links

> [!INFO]
>
> - An [essential resource for getting started if you have never worked with shaders (The Book of Shaders)](https://thebookofshaders.com).
>
> - An [overview of the language specifications in video.](https://www.youtube.com/watch?v=uOErsQljpHs)
>
> - The [official documentation of the language](https://registry.khronos.org/OpenGL/specs/gl/GLSLangSpec.4.60.pdf ) available on the Khronos website as a downloadable PDF.
>
> - An index of [functions included in the GLSL standard library](https://docs.gl/sl4/sin).
>
> - More details on the [implementation of GLSL TOP](https://docs.derivative.ca/Write_a_GLSL_TOP) (the underlying operator of Pixel Wrangle) in Touchdesigner.
>
> - [Shadertoy](https://shadertoy.com), a website that allows for the creation of shaders online using WebGL. It is a great place to find many examples of shaders created by the community.
>
> - [The website of Inigo Quiles](https://iquilezles.org/articles/) is also a massive learning resource for understanding various mechanisms related to shader creation and the underlying math.
>
> - [Lygia](https://lygia.xyz) is a community project led by Patricio Gonzalez Vivo (The Book of Shaders) among others, and is a library of GLSL (and HLSL) functions that provides a large number of very useful functions. It is natively provided with Pixel Wrangle and greatly facilitates work.
>
> - Many YouTube videos cover shader creation. Some in the context of Touchdesigner, Unity, Shadron, glslsandbox, or other frameworks. The YouTube channel The Art Of Code, for example, offers a good introduction to GLSL on Shadertoy through practice.

## Rendering Pipeline

GLSL is a language that can be used at different stages of the rendering pipeline.

Each stage of the pipeline has its own variables and functions.

Here is a list (non-exhaustive and probably inaccurate in some places) of the stages of the pipeline, in chronological order:

- Vertex Specification: This is the initial stage, which involves retrieving the list of points and primitives and their attributes (ID, position, etc.) from CPU memory and converting them into a format that can be used in the subsequent stages of the rendering pipeline in GPU memory. This step is automatically performed by Touchdesigner and the OpenGL/Vulkan backend when using a [GLSL TOP](https://docs.derivative.ca/GLSL_MAT) on a [Geometry COMP](https://docs.derivative.ca/Geometry_COMP).

- Vertex Shader: This is a program that operates at the level of the geometry of a 3D object, and is executed simultaneously on each vertex of the input geometry. It allows, in particular, to create and modify attributes such as the position of points to deform an object, for example. The result of these operations is then passed on to the next stage of the rendering pipeline. Some intermediate steps can be performed at this stage (see Geometry Shaders, Tessellation Shaders...).

- Rasterization: Rasterization is an operation that transforms a scene described in a 3D space into a 2D image that can be displayed on a screen at a chosen resolution (discretization). You can imagine this step as the projection of our screen space (the view) onto the 3D objects in the scene. This operation is automatically and transparently performed by the OpenGL/Vulkan backend. There is no associated shader that allows for control over this stage of the pipeline, but it is crucial in the rendering pipeline.

- Pixel Shader (or Fragment Shader): This is a program that runs on each pixel rendered on the screen. A pixel shader allows you to manipulate this data to produce the final color of the pixels.

-> In the case of a 3D object from a vertex shader, the Pixel shader performs an interpolation of the attribute values provided by the vertex shader. You can then manipulate them as you wish to obtain the desired result

-> In the case where no Vertex Shader is provided. Touchdesigner's Pixel shader implementation (GLSL TOP) provides 2 rectangle triangles by default that form a rectangle with the exact dimensions of the screen. You therefore have a coordinate system from which you can have fun. Note that it is quite possible to create 3D images without using a vertex shader or a geometry composed of points. This means that all the logic must be coded in the pixel shader. Raymarching (or 'Sphere Tracing') is one of these techniques. It is based on the description of shapes by distance fields called SDFs. You can find a lot of examples on [Shadertoy](https://shadertoy.com) and [Inigo Quiles's website](https://iquilezles.org/articles/) if you want to learn more about these techniques. As for raymarching in Touchdesigner, the rayTK project led by tekt is a wonderful example of porting this technique to the Touchdesigner environment. It greatly simplifies its use through a pre-constructed operator ecosystem. A tip, go see it!

In Touchdesigner, GLSL can be used through [GLSL TOP](https://docs.derivative.ca/GLSL_TOP) (Image generation in the TOP context) or [GLSL MAT](https://docs.derivative.ca/GLSL_MAT) (Material generation for 3D objects).

NB: The work environment provided by Pixel Wrangle currently only works with [GLSL TOP](https://docs.derivative.ca/GLSL_TOP).

## Pixel Shader

In the case of a pixel shader without a manually associated vertex shader (the default behavior of Pixel Wrangle), the thing you are actually manipulating within the main() function of your shader is the current pixel.

This pixel has an intrinsic and fundamental characteristic, its position in screen space.
In the case of a 2D image, for example, this is the x and y coordinates.

It is important to understand that this same program is executed simultaneously on each of the pixels that make up your final image.

For an image of 1024 *1024 pixels, for example, this program will be executed 1024* 1024 times, a total of 1,048,576 times. That's a lot.

In the case of a standard processor (CPU), all these executions would be carried out sequentially (or at best, distributed across the few cores of your CPU).

A CPU is very efficient for most everyday tasks with a computer and has fast and significant memory storage (RAM), but it is not very efficient when you need to perform the same processing on a large number of elements at the same time, as is the case with an image made up of pixels.

On the contrary, a shader runs on your graphics processor (GPU). GPUs have a different architecture than CPUs, which is designed to execute a large number of tasks at the same time.
For comparison, a GPU has a few thousand cores to more than 10,000 for high-end GPUs.
This greatly reduces the number of operations performed at the same time, as the calculations are distributed among the thousands of cores of your GPU.

The main limitations of the GPU:

- Limited memory space (VRAM is expensive and of fixed size), which implies a certain size and complexity limit for operations, more easily reached than with a CPU.
- A lower sequential execution speed (per GPU core) than your CPU.
- Current GPUs perform calculations with numbers only, making them difficult to use for tasks that manipulate strings of characters or certain types of complex data that are common in traditional programming.
- Abstraction capabilities are reduced, the scope of operations is limited.

### GPU vs CPU by example

**CPU** :

- Write 2 nested for loops, each iterating over the x and y coordinates of the image.
- Execute your main() function with all your code inside the 2 loops, on each of the pixels composing your image.

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

- The coordinates are already provided by the main context through a variable called vUV in Touchdesigner.
- Just write your code in the main() function and retrieve the coordinates directly from the shader (vUV).
This would have the following form for an image of 1024 * 1024 pixels: Within the main() function of a pixel shader, you can consider that each of your instructions is executed simultaneously, and the execution context is the internal part of this double 'for' loop.

GLSL and Vulkan intelligently distribute the simultaneous execution of the program on all available cores of your GPU.

### Pixel Coordinates

In Touchdesigner, the normalized x and y coordinates are provided to you in the form of a vector in your shader through a variable directly provided by the execution context.

This is the variable [vUV](https://derivative.ca/UserGuide/Write_a_GLSL_TOP#Pixel_Shaders)

```glsl
vec2 coords = vUV.xy; // Will store a copy of current XY coord in 'coords' vec2 variable
```
