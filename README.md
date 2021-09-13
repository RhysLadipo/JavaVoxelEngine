### Hi there, I'm Rhys 👋

[![Website](https://rhysladipo.github.io/)]

## I'm a JAva Developer & a Graphic Designer.

- 👯 I’m looking to collaborate with other content creators.
- 🥅 2021 Goals: Contribute more to Open Source projects

##

<br />

### Languages and Tools:

<img align="left" alt="Visual Studio Code" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/visual-studio-code/visual-studio-code.png" />
<img align="left" alt="HTML5" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/html/html.png" />
<img align="left" alt="CSS3" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/css/css.png" />
<img align="left" alt="JavaScript" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/javascript/javascript.png" />
<img align="left" alt="React" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/react/react.png" />
<img align="left" alt="SQL" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/sql/sql.png" />
<img align="left" alt="Git" width="26px" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/git/git.png" />
<br />

---

### 📺 Latest Project Java Voxel Engine 

Part 1

Java Voxel Engine 

Using The Programmable OpenGL3+ Pipeline in Java.

Here I'm using the Eclipse IDE to code the entire voxel engine in Java.
For the OpenGL binding I'm using the Light Weight Java Game Library (LWJGL).

https://www.lwjgl.org/

https://www.opengl.org/

The Light Weight Java Game Library was also used to develop a voxel world on the original Minecraft.


*VoxEngine1


Create :- Set Up a the project & display - Created the Render Engine & Display Manager & Main Game Loop - Renderer- Added Key Down Logic to exit.


Part2

Rendering the first quad.


*VoxelEngine 1stQuad 2


The first quad is rendered onto the screen. 

OpenGL Coordinate system allows us to store verteces to memory.


*VoxEngine OpenGL CoordinateSystem 3

The centre of the screen is (0, 0). The top left coner is (1,1) 1 in X and 1 in Y, the bottom right is (1, -1) 1 in X and -1 in Y, the bottom left will be 
(-1, -1) -1 in X and -1 in Y and the top right should be (-1, 1) -1 in X and 1 in Y.


*VoxEngine The Quad 4


(0.5, 0.5) gives us a vericey half way between the centre point and the top right corner.


In games rendering is done in triangles. To render a quad two triangles are used. 
Note they share two vertices - The bottom right (0.5, -0.5) and top left (-0.5, 0.5). This makes rendering more efficient.


*VoxelEngine VerticesInMemory 5


We store the vertices into a buffer or Vertex Buffer Object (VBO). All model data will be stored in a VBO.
A VBO is essential a buffer that can store an array of floats or integers.

Vertex Array Object (VAO)


*VoxelEngine VAO 6

A VAO can store any number of attributes 1, 2, 3, .... Here we store the first VBO in index 0 of the VAO

*VoxelEngine VAO 7

*VoxelEngine VAO & VBO 8

First thing to do is create a VAO using an OpenGL function called glGenVertexArrays. This will create a VAO and return its I.D.
In order to use the VAO it has to be bound by calling glBindVertexArrays. Once bound you create a VBO by calling the glGenBuffers function.
This creates the VBO and returns its I.D. Once created it has to be bound with the glBindBuffers function. Then store the data in the VBO using the glBufferData function.
Once the data is in the VBO its time to store the VBO into the VAO using the glVertexAttribPointer function. Once your finished storing data into them and you've finished 
with the VBO and VAO its time to unbind them so openGL knows you've finished with them.

New Models package created and new class made called RawModel to store the vertices.

Generated getters and setters for the VAO ID & vertexCount variables prior to rendering. Then created a new class in the RenderEngine called the Loader
which will be in charge of loading anything into memory such as models or pictures. Creat a method to return a VAO and return its I.D. Then created another method to store
storeDataInAttributeList to store all data in a VBO and then store the VBO inside atribute '0' of the VAO. The final method is the cleanUp() method.The data cannot take in a float array 
first as it has to be stored in a float buffer first. So we create a method to store a float array in a float buffer - storeDataInFloatBuffer(float[] data) - Then we 
create the loadToVAO(float[] vertices) method. This creates and binds a VAO and storeDataInAttributeList(vertices, 0, 3). Finally we unbind the VAO by calling glBindVertexArray(0) and 
initiate the cleanUp() method. 

The Entity Renderer Class.

Then a new class EnityRenderer is created to render the models. This contains the render(RawModel model) method. glDrawArrays draws the verteces to the screen.
In the masterRenderer we create a render(RawModel model) method. A new instance of the loader class (imported) is created in the MainGameLoop and create a float array of verteces.


-0.5f, 0.5f, 0,
-0.5f, -0.5f, 0,
0.5f, -0.5f, 0,
0.5f, -0.5f, 0,
0.5f, 0.5f, 0,
-0.5f, 0.5f, 0


Part 3

Improving the Rendering System & using Shaders.

. Improve rendering system by rendering with index buffers.

. Utilise Shaders.


*Rendering With IndexBuffers 9


Instead of giving six vertices for the two triangles we now give 4. 


-0.5f, 0.5f, 0,
-0.5f, -0.5f, 0,
0.5f, -0.5f, 0,
0.5f, 0.5f, 0,

*Rendering With IndexBuffers 10

*Quad Rendered To Screen 11

Create another method the bindIndicesBuffer(int[] indices) and create a vbo & bind it. Call glBufferData(GL_ELEMENT_ARRAY_BUFFER, buffer, GL15.GL_STATIC_DRAW_).
But first we need to store the array in a IntBuffer = BufferUtils. Create buffer - IntBuffer buffer = storeDataIntBuffer(indices).

Change the way we render models in the EntityRenderer with glDrawElements(GL11.GL_Triangle, indices_count, type, indices_buffer_offset);

Declare the indices in the MainGameLoop.

int[] indices = {
	
	0,1,2,
	2,3,0

};

This is a much more efficient way of rendering data that takes up allot more space in memory.


SHADERS?


*VoxEngine Shaders 12


CPU V GPU

The CPU executes all of the Java code, game logic and all of the calculations within the game.
The GPU is used with OpenGL. So for any draw call such as GL_Draw_ELEMENTS the GPU will execute that function.
The GPU also executes Shader code and does anything to do with graphical rendering of the game.

OpenGL3

The old OpenGL 1& 2 had built in lighting functions. OpenGL3 adds includes a programmable pipeline which made shaders available for the first time.
The old openGL had a fixed function pipeline so was not very flexible however openGL3 and removed all the old fixed function pipelines so we have
to program the lighting functions our selves which involves allot more code to achieve something very simple. However it does give us much more flexibility to 
change code & rendering things as we want to.

The GPU does not have access to any Java code so the GPU must be programmed using Shader code. 

What are Shaders?


*VoxEngine Shaders 13

Shaders are programs executed on the graphics card GPU and determine the colour of every pixel on the screen. Shaders are programs created in 
text files that are loaded into OpenGL where they are executed. Shaders are used to create things like a 3D View, transformations on vertices,
all the lighting, shadowing, post processing effects, fog, texturing calculations.

Shaders are programmed in a language called GLSL(Graphics Library Shading Language).
This menas we have complete flexibility on how graphics are rendered.


*VoxEngine Shaders 14

There are two kinds of shaders. "A Vertex Shader," and a ,"Fragment Shader."

Vertex Shader.

The vertex shader executes once for every verticex, determines the position of where every vertex should be rendered onto the screen.

As shaders are programmed in a different language it is difficult to communicate between JAva code and shaders. As in Java shaders also have 
variables and there are two ways of passing variables between shaders and java code. The vertex shader can have an input variable from the VAO or Uniform.
Uniform variables are used to load variables from Java code to the shader. GL_Uniform...function that allows loading of variables.


Fragment Shader.

This deternmines the colour of every pixel and object. Input variables are outputs from the vertex shader.


*VoxEngine Shaders 15


GLSL Vertex Shader & Fragment Shader code requires you first declare what version of GLSL you are using :- #version 400 core. (Version 4)
Then the input and output. GLSL code is very similar to Java and very easy to work with.


Part 4

Create the shaders package and another class called the shadersProgram. This class is going to be a master class or parent class that means that 
its going to have children classes linked to it. These children classes will inherit some of its methods. The Shader Program class will load in 
text files from strings. Shaders go in text files like:-


*VoxEngine Shaders 16


First thing we do is load a text file as a string with StringBuilder shaderSource = new StringBuilder()
A StringBuilder is a string the only difference is that you can call it "shaderSource" & append things to it shaderSource.append("d");
(Or a float or boolean to this).
 
Then an input stream builder is created which will read in the file.

InputStream in = Class.getResourceAsStream(file);

Then create a new BufferedReader called reader to read the file. 

The following need to be imported:

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;

import org.lwjgl.opengl.GL20;

Now we have a reader that can read in the text files line by line!

String line;
while ((line = reader.readLine()) != null) {
	shaderSource.append(line).append("//\n");


To separate the lines use .append("//\n") to tell the string you are at the end of the line & it has to go to the next line.

Surround with a try catch and close the reader & exit the game.

We are now reading a file line by line and storing it in the shaderSource variable.


Error Checking

Error checking by checking shader status with GL20.glGetShaderi() to make sure the game compiled & is not stuck in a loop.








