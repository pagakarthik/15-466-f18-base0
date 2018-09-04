# Game Information
[Forked from ixchow](https://github.com/ixchow/15-466-f18-base0)

## Development Environment

Ubunutu 16.04

Assets generated in Blender 2.79b

### Title: Save the ancestors

### Author: Karthik Paga

### Design Document: [Stonehenge](http://graphics.cs.cmu.edu/courses/15-466-f18/game0-designs/apnayak/)

## Screen Shot:

![Screen Shot](screenshots/save_ancestors.png)

## Design of Game environment:

![Landing](screenshots/stonehenge_design.png)
![Locate the duck and alien](screenshots/stonehenge_design_1.png)
![Kill the alien in RED](screenshots/sthng_design_2.png)
![But don't kill the duck](screenshots/sthng_design_4.png)


## Difficulties Encountered:

Navigating in Blender to acces object properties and change the settings was quite a hassle. 
Getting used to "shortcuts" in Blnder and actually making the chnages was 10-15 times more time 
consuming than understanding the code. Once a game compiled, I was not any mor einterested in 
going back to blender to make "unknown" changes to have a full-fledged game environments. 
For exmaple, the statues necessary for the stonehenge environment - would always get upscaled no matter
updated and saved dimensions. 

## What was achieved

    - All the assets are mine! :P
    - The vertex colors were added and successfully used as part of the game environment
    - A non default "game environment" that actually compiles and works
    - The assets are all 3D and have associated colors
    - Several simple and complex assets have been created
    - And the requirment to set and use vertex colors, compiled code and a game have been fulfilled.

## Bottlenecks:
    
    - If the assets when imported and visualized made sense
    - (Sorry, but) nothing in Blender really made sense! - "shortcuts" are not shortcuts and the quick access
        menus are just dumped on the landing page

## What was awesome:

    - ftjam was quite good!
    - the project itself was setup and managed with directions to setup and use the libraries was hassle free - Thanks Jim.

## Good (Necessary) Code:

    ```
       mesh.vertex_colors['Col'].data[i].color
    ```

After having working through the documentation of blender in order to associate the vertex colors and then
finding a means to access the content for generating the blob, this line of code was an important milestone.
And also considering that  none of the assets used in the game are the default kind, this line of code has been
really well thought through and also experimented - for the given code settings. 

References:
[Blender-python-vertex colors](https://blender.stackexchange.com/questions/909/how-can-i-set-and-get-the-vertex-color-property)

# Using This Base Code

Before you dive into the code, it helps to understand the overall structure of this repository.
- Files you should read and/or edit:
    - ```main.cpp``` creates the game window and contains the main loop. You should read through this file to understand what it's doing, but you shouldn't need to change things (other than window title and size).
    - ```Game.*pp``` declaration+definition for the Game struct. These files will contain the bulk of your code changes.
    - ```meshes/export-meshes.py``` exports meshes from a .blend file into a format usable by our game runtime. You will need to edit this file to add vertex color export code.
    - ```Jamfile``` responsible for telling FTJam how to build the project. If you add any additional .cpp files or want to change the name of your runtime executable you will need to modify this.
    - ```.gitignore``` ignores the ```objs/``` directory and the generated executable file. You will need to change it if your executable name changes. (If you find yourself changing it to ignore, e.g., your editor's swap files you should probably, instead be investigating making this change in the global git configuration.)
- Files you probably should at least glance at because they are useful:
    - ```read_chunk.hpp``` contains a function that reads a vector of structures prefixed by a magic number. It's surprising how many simple file formats you can create that only require such a function to access.
    - ```data_path.*pp``` contains a helper function that allows you to specify paths relative to the executable (instead of the current working directory). Very useful when loading assets.
	- ```gl_errors.hpp``` contains a function that checks for opengl error conditions. Also, the helpful macro ```GL_ERRORS()``` which calls ```gl_errors()``` with the current file and line number.
- Files you probably don't need to read or edit:
    - ```GL.hpp``` includes OpenGL prototypes without the namespace pollution of (e.g.) SDL's OpenGL header. It makes use of ```glcorearb.h``` and ```gl_shims.*pp``` to make this happen.
    - ```make-gl-shims.py``` does what it says on the tin. Included in case you are curious. You won't need to run it.

## Asset Build Instructions

In order to generate the ```dist/meshes.blob``` file, tell blender to execute the ```meshes/export-meshes.py``` script:

```
blender --background --python meshes/export-meshes.py -- meshes/meshes.blend dist/meshes.blob
```

There is a Makefile in the ```meshes``` directory that will do this for you.

## Runtime Build Instructions

The runtime code has been set up to be built with [FT Jam](https://www.freetype.org/jam/).

### Getting Jam

For more information on Jam, see the [Jam Documentation](https://www.perforce.com/documentation/jam-documentation) page at Perforce, which includes both reference documentation and a getting started guide.

On unixish OSs, Jam is available from your package manager:
```
	brew install ftjam #on OSX
	apt get ftjam #on Debian-ish Linux
```

On Windows, you can get a binary [from sourceforge](https://sourceforge.net/projects/freetype/files/ftjam/2.5.2/ftjam-2.5.2-win32.zip/download),
and put it somewhere in your `%PATH%`.
(Possibly: also set the `JAM_TOOLSET` variable to `VISUALC`.)

### Libraries

This code uses the [libSDL](https://www.libsdl.org/) library to create an OpenGL context, and the [glm](https://glm.g-truc.net) library for OpenGL-friendly matrix/vector types.
On MacOS and Linux, the code should work out-of-the-box if if you have these installed through your package manager.

If you are compiling on Windows or don't want to install these libraries globally there are pre-built library packages available in the
[kit-libs-linux](https://github.com/ixchow/kit-libs-linux),
[kit-libs-osx](https://github.com/ixchow/kit-libs-osx),
and [kit-libs-win](https://github.com/ixchow/kit-libs-win) repositories.
Simply clone into a subfolder and the build should work.

### Building

Open a terminal (or ```x64 Native Tools Command Prompt for VS 2017``` on Windows), change to the directory containing this code, and type:

```
jam
```

That's it. You can use ```jam -jN``` to run ```N``` parallel jobs if you'd like; ```jam -q``` to instruct jam to quit after the first error; ```jam -dx``` to show commands being executed; or ```jam main.o``` to build a specific file (in this case, main.cpp).  ```jam -h``` will print help on additional options.
