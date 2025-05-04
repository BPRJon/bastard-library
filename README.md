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

## function objHandler.URLLoadPropMeshMaterial( pos, ang, freeze, propurl, meshurl, maturl, callback )

loads a custom prop and sets it mesh and mesh material to custom ones

- pos (vector)
    - position you want the prop to spawn at
- ang (angle)
    - angle you want the prop to face upon spawn
- freeze (bool)
    - whether or not the prop is frozen upon spawn
- propurl (string)
    - string url to the custom prop obj
- meshurl (string)
    - string url to the mesh obj
- maturl (string)
    - string url to the material file
- callback (function)
    - has one argument, ent, that will give back the custom prop. to get the prop as a var put function(ent) var = ent end

# Behavior Tree

# Base Functions

## Branch Types

BRANCHTYPES = {
    SELECTOR = 1,
    SEQUENCE = 2
}
the different branch types that the tree can handle

Selectors will pick the first child node that succeeds and ticks it

Sequencers will tick all children unless one child fails

## Node States

NODESTATUS = {
    IDLE = -1,
    RUNNING = 0,
    FAILED = 1,
    SUCCEEDED = 2
}

the current status of the node

Idle - has not been run yet
Running - Is in the process of checking itself for success for failure
Failed - Failed its check
Succeeded - Succeeded its check

## Leaf Action States

ACTIONSTATUS = {
    IDLE = -1,
    RUNNING = 0,
    COMPLETED = 1
}

Idle - has not been run yet
Running - Is in the process of completing the action
Completed - Action is complete

## behaviorTree.createRootNode( blackboard )
makes a root node to begin building a behavior tree. a root node can only have ONE branch or leaf connected to it
the connected node will always succeed because otherwise the tree wouldn't be able to work

- blackboard (table)
    - the table you would like to supply to the tree. this table acts as the tree's 'memory', storing important variables for easy use
    
== RETURNS ==

- root (RootNode)
    - The root node


# RootNode Functions

## RootNode:getBlackBoard()
get the blackboard for you to be able to read or modify

== RETURNS ==

- blackboard (table)
    - The root node's blackboard
    
## RootNode:setBlackBoard( key, value )
sets the blackboards key to that value

- key (string or number)
    - the key/index of the blackboard you want to change
- value (any)
    - the value to set that key to

## RootNode:tick()
make the root node begin a tick down the tree

## RootNode:addBranch( branchtype )
a branch node is a decision node, and will either attempt to activate all children until ONE succeeds (selector) or will only succeed if all children can succeed (sequence)

- branchtype (number)
    - the type of branch you want to add, will automatically succeed. see BRANCHTYPES.
    
## RootNode:addLeaf( callback )
a leaf node is essentially an action node, whatever you'd like the tree to execute put into the callback and the leaf will automatically run it

- callback (function)
    - the function to run when the leaf succeeds. a leaf connected to a root will automatically succeed


# BranchNode Functions

## BranchNode:addBranch( branchtype, index, checkfunction )
adds a branch node as a child to this branch node

- branchtype (number)
    - the type of branch you want to add. see BRANCHTYPES.
- index (number)
    - essentially acts as the priority order for being run, in ascending order.
- checkfunction (function)
    - this function will need to change the node's status based on it's current progress. This function will control whether or not to continue down this path. see NODESTATUS
        
## BranchNode:getBlackBoard()
returns the blackboard table

== RETURNS ==
- blackboard (tbl)
    - the blackboard of the root node
    
## BranchNode:setBlackBoard( key, value )
sets a value in the blackboard from the branch

- key (string or number)
    - the key position of the table to want to modify
- value (any)
    - the value to be set in the table
    
## BranchNode:getStatus()
returns the number status of the node. see NODESTATUS

== RETURNS ==

- status (number)
    - current status of the node
    
## BranchNode:setStatus( status )
sets the status of the node according to NODESTATUS

- status (number)
    - the status you want to set the node to
    
## BranchNode:addLeaf( index, checkfunction, callback )
a leaf node is essentially an action node, where once the tree gets to this node actual actions can be taken

- index (number)
    - essentially acts as the priority order for being run, in ascending order.
- checkfunction (function)
    - this function will need to change the node's status based on it's current progress. Has one input, leaf, which is the created node. see NODESTATUS
- callback (functions)

# LeafNode Functions

## LeafNode:getStatus()
returns the status of the leaf node

== RETURNS ==

- status (number)
    - current status of the node. see NODESTATUS
    
## LeafNode:setStatus( status )
sets the status of the leaf node

- status (number)
    - the number status you wish to set this node's check to. see NODESTATUS

## LeafNode:getActionStatus()
returns the current action status of the leaf

== RETURNS ==

- status (number)
    - the current status of the leaf. see ACTIONSTATUS

## LeafNode:setActionStatus( status )
sets the current action status of the leaf

- status (number)
    - the current status of the leaf. see ACTIONSTATUS
    
## LeafNode:getBlackBoard()
returns the root's blackboard

== RETURNS ==

- blackboard (table)
    - the root node's blackboard
    
## LeafNode:setBlackBoard( key, value )

- key (string or number)
    - the key position of the table to want to modify
- value (any)
    - the value to be set in the table
    
# Tick and Check functions

Branches and Leaves have :check() and :tick() functions but you technically should never need to use them, but you might have some edge case you want to fire a node off so I will quickly explain the difference in the two functions:

node:check() will run the check function of the node, and will just likely set it's node status if you've correctly set up its check functions
    
node:tick() will run the node's children in the case of a branch, and the callback in the case of the leaf. this is a way to bypass checking down the tree if say you need someone to do something instantly and skip the tree temporarily