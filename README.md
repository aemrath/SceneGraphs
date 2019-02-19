# SceneGraphs
1. A scenegraph is just a tree-like data structure, there is a root of an object at the top and every other 
object that exists is added to a tree.  There can be multiple nodes in the tree like lighting, a camera, or 
transformations.  The code provided to us to use scenography has an interface for INode, IScenegraph, and 
IScenegraphRenderer.  AbstractNode implements INode and this is the class where all the default methods are 
implemented. From AbstractNode, LeafNode, TransformNode and GroupNode extend AbstractNode.  GroupNode is used 
to group together other nodes and has an arbitrary number of children. LeafNode will be a leaf of the scenegraph. 
This is the kind of node that will undergo geometry for rendering.  TransformNode represents a node of 
transformation in the scene graph, and it has only one child for changing the coordinate system.  
The GL3ScenegraphRenderer class implements the IScengraphRenderer interface and works with the JOGL library to
draw what we need properly.
From these nodes, the Scenegraph class implements the IScenegraph interface and it is the class that contains the root,
a map of nodes and textures, and a renderer (in this case an IScenegraphRenderer).  The Scengraph class brings 
everything together and uses the SceneXMLReader to read from the xml files and properly draw the correct objects.  
The View class, JOGLFrame, etc. are similar to previous examples and assignments in that the View class is the controller 
of the program and will make calls to corresponding methods to take a specific scenegraph and draw it.

2. The Scenegraph class houses all of the data the program will need to draw the image, the polygon meshes,
root, and nodes.  The View class contains a Scenegraph and will initialize the scenegraph, get all of its meshes,
roots, nodes, etc and set a renderer to draw the data.  In the draw method, we call scenegraph.draw on the modelView 
stack of matrices so that the scenegraph will call on all of its nodes to be drawn and the tree-like structure of the
scenegraph will be fully evaluated.  The transformations get applied to the correct parts because of the way we are using
a stack to store our modelview matrix transformations and the tree structure provides certainty that every part of 
the structure will be reached.

3. The GL3ScenegraphRenderer class is used to work with the JOGL library, it holds JOGL specific rendering context,
shaderLocations,  and a table for shader variables -> vertex attribute names, a table for textures, and a table for 
renderers for each mesh object.  It makes sure that the shader locations have been set before drawing with a boolean.  
It’s functionality includes checking if the rendering context is correct (JOGL specific), adds textures and mesh to draw later.  Draws the scenegraph starting at its root, and draws a mesh at its specific shader location.  It also handles initializing the shader program and cleanup.  To create a textual rendering of the scenegraph you would need to keep text data in each node, and then use a renderer like this one, or a like the textRenderer in previous examples, that draws the text similar to how GL3ScenegraphRenderer currently has a table of meshes, and adds the mesh and draws it, you could have the text data, add it and draw it.
