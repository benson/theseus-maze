Random maze generation 
Djikstra's/A* pathfinding
Simple avoidance AI

maze.html is preconfigured to give a semi-interesting setup.
randomMaze.html...is just random.  Could be interesting, could be boring.

At the top of these, there are parameters that can be changed for maze size and map scale (blocksize).
Feel free to play around with these, however, you might have to change the canvas size if you make a 
very large map without scaling it down appropriately.

press "a" to generate A* path to goal - it will be highlighted.
press "b" to generate BFS (Djikstra's) path to goal - it will be highlighted.
press "s" to create a spider.
press "g" to display (on the console) how many nodes were visited by the most recent path.
press "space" to move one step in the simulation.  if no path has been created for
    theseus, nothing will happen.
   
Open the javascript console (control-shift-j in chrome) to see some debugging info (A* calc time, for example).


Pink spiders are small
Orange spiders are medium
Red spiders are large


Spider Behavior Tree:

        can see other spider?
       yes /            \ no
          /              \
 are you smaller??      move randomly
   yes /       \ no
      /         \
 run away     can you pounce?
             yes /      \ no
                /        \ 
            pounce      move closer
         
Theseus Behavior Tree:
        
        can see spider?
      yes /         \ no
         /           \
   run away       continue on path
               
   
Spider roams randomly until seeing another unit within 4 spaces.
If it sees a larger spider, it will run.
If it sees a smaller spider, it will hunt.
If it sees Theseus, it will hunt.
If it is hunting, and within 2 spaces of Theseus or a smaller spider, it will pounce.

Theseus' behavior is simple.  He follows his path to the goal, only deviating if there is a spider within 3 spots.  He makes sure
that he isn't deviating to a spot that also has a spider within 3 spots.

1. Maze is always solveable because of how it is generated:
a. First, a default maze of alternating wall/corridor rings is created.
b. Second, random wall segments are added in corridor spaces.
c. Third, "changeMaze(startPoint)" is called.
 - This method begins at the start point of the maze.
 - It marks startPoint as having been visited.
 - It gets all unvisted adjacent spaces (assuming 4-way movement) to the given startPoint.
 - If a certain counter is up, it picks a random unvisited adjacent space.
 - If this counter is not up, it prioritizes corridors over walls.
     - If the picked space is a corridor, it recursively calls changeMaze on that new point,
        and then on all other adjacent points.
     - If the picked space is a wall, it checks to see if the wall has a space behind it
        - if it does, it makes a hole in this wall
        - if it doesn't, no hole is made
 - this method is eventually called on all points.
 - because all points must be visited, the center (exit) must be visited.
 - to get to the center, all walls between the entrance and exit must be crossed at least once.
 - this means a hole must have been made in each wall at least once.
 - because new WALLS are generated BEFORE this algorithm begins, no wall can block off a 
    path that has already been created.
 - therefore, a path from entrance to exit will always exist.



Both seedrandom.js and priority_queue.js are used.  I did not create either, and licensing information
for each is available in their respective files.
