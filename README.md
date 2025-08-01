# bastard-library
A collection of StarFall Libraries ive made to make my (and possibly your) life easier

# OBJ Handler V2

very much imrpoved mesh loading

////// SHARED //////

## convertHolosToMeshes( holotbl, objlink, clientcallback, maxcpu or nil )

A shared function doesnt make much sense so let me quickly explain before telling you what each var is/does.
You will have to run this function twice, once on the server and once on the client, and the vars for both are slightly different.
    
It should look something like this:

```
if SERVER then
    convertHolosToMeshes( holotbl, objlink )
else
    convertHolosToMeshes( nil, objlink, clientcallback, maxcpu or nil )
end
```

### VARS

 - holotbl [SERVER] (table)
     - A table of hologram entities you want to turn into meshes
 - objlink [SHARED] (string)
     - A link to the obj file you want to turn into a mesh, must be pre-triangulated
 - clientcallback [CLIENT] (function)
     - A function that will automatically be run. This function has 2 inputs: holograms (table) and obj (table).
         - holograms is the holotbl sent to the client automatically by the function
         - obj is a table of Mesh objects created from your OBJ file
 - maxcpu [CLIENT] (number)
     - a number 0-1 representing how much of the client CPU the function can take to generate the mesh (default 0.25)

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