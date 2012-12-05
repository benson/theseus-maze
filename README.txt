press "a" to generate A* path to goal - it will be highlighted.
press "b" to generate BFS (Djikstra's) path to goal - it will be highlighted.
press "s" to create a spider.
press "g" to display (on the console) how many nodes were visited by the most recent path.
press "space" to move one step in the simulation.  if no path has been created for
    theseus, nothing will happen.
   

There are 4 given files: 
A3Final.html - this shows Theseus with his AI, spiders with their AI, and a nice-looking maze.
             - Try adding many or few spiders to the maze - depending on how many, Theseus may or may
                not be able to make it to the goal.
A3noTheseusAI.html - this is the same as A3Final.html, but Theseus has his AI turned off.  He should just
                    - run towards the goal and be killed by spiders (if you choose to add any).
      
A3Random.html - this is not a seeded map, so it will generate random maps upon each refresh.

A3Large.html - shows how an extremely large map can be generated.

At the top of these, there are parameters that can be changed for maze size and map scale (blocksize).
Feel free to play around with these, however, you might have to change the canvas size if you make a 
very large map without scaling it down appropriately.

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

1. My maze is always solveable because of how it is generated:
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


Graphs of results of A* vs Dijkstra's are included.

In my code, I left in my commented-out version of Dijkstra's (which is really DFS, since all distances
between points are 1).  It ran MUCH faster than my A* despite exploring twice as many nodes.  This is
due to the fact that Dijkstra's does not use a priority queue.  The priority queue model allows for
A* to explore better paths first, but at the expense of maintaining a constantly sorted list.  

To show the difference between A* and Dijkstra's, I switched my Dijkstra's to exactly my A* algorithm,
but with 0 as its heuristic.  This means both A* and Dijkstra's use the same priority queue structure.
This shows that the heuristic does in fact result in better performance.


Both seedrandom.js and priority_queue.js are used.  I did not create either, and licensing information
for each is available in their respective files.
