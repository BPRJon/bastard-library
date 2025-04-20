# bastard-library
A collection of StarFall Libraries ive made to make my (and possibly your) life easier

# OBJ Handler

## //// SERVER ////


## objHandler.createPropFromFile( pos, ang, path, frozen )
loads a sf custom prop from an obj file in your sf_filedata folder


- pos (vector)
    - vector position the prop should spawn from
 - ang (angle)
    - angle the prop should face when spawned
- path (string)
    - string path to the obj file local to your sf_filedata folder
- frozen (bool)
    - whether or not the prop is frozen upon spawn



## objHandler.cachePropVerticies( path )
saves the vertecies needed for creating a custom prop in your sf_filedata/obj2prop/ folder for easier future use. deserialize and enjoy


- path (string)
    - path to the obj file you want to cache relative to sf_filedata/



## objHandler.URLLoadObjAndMaterial( objurl, materialurl )
takes mesh and material urls and automatically creates holograms for each obj object and appies the mesh and material
NOTE: uses a 025x025x025 default cube model as the base holo, meaning the textures HAVE to be 1024^2 and the render bounds will need to be set manually.

- objurl (string)
    - string url for loading the OBJ to be used as the visual mesh
- materialurl (string)
    - string url for loading the custom texture that will be applied to the mesh
    
==RETURNS==

- return_tbl
    - table where each key is the object number of the mesh and the value is the hologram entity
