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

saves the vertecies needed for creating a custom prop in your sf_filedata/obj2prop/ folder for easier future use
deserialize and enjoy


- path (string)
    - path to the obj file you want to cache relative to sf_filedata/



## objHandler.URLLoadObjAndMaterial( objurl, materialurl, callback )

takes mesh and material urls and automatically creates holograms for each obj object and appies the mesh and material

- objurl (string)
    - string url for loading the OBJ to be used as the visual mesh
- materialurl (string)
    - string url for loading the custom texture that will be applied to the mesh
- callback (function)
    - has one argument, tbl, that will give back the table of holograms. to get the table as a var put function(tbl) var = tbl end



## objHandler.propFromURL( pos, ang, url, freeze, callback )

load an obj from a url and automatically turn it into a custom prop

- pos (vector)
    - position you want the prop to spawn at
- ang (angle)
    - angle you want the prop to face upon spawn
- url (string)
    - url to the OBJ to want to load
- freeze (bool)
    - whether or not the prop is frozen upon spawn
- callback (function)
    has one argument, ent, that will give back the custom prop. to get the prop as a var put function(ent) var = ent end
