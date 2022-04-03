# Raycast Projectile System

## Overview
This is a system which uses OOP, raycasting, and a physics algorithm that I learnt through online resources. It works by first creating an object using the ProjectileComponent; which is then inserted into a ProjTable to be rendered every frame. During the update, the projectiles within the table will have their positions updated through a physics algorithem. The projectiles will also cast a ray (With an ignore list) at every update in order to check for collisions. Once it finds a collision, it will go through a series of checks to identify the type of object it has collided with. In my case, there were 3 types of objects which were tagged "Obstacle", "Zombie", and "ZombieDead". If the object was tagged as "Zombie" it would call ProjectileService to replicate the damage taken onto the server so it is replicated across all players. That's pretty much the essence of how the system works.

You may see it within the code written in this repo, that some of it relies on getting game object data from the *GameDictionary*. This is because I like to keep all of my game object data within one dictionary so it can be easily accessed and read. Take a look at the *GameDictionary* yourself if you're interested how I handle game object data!

## Physics Algorithm
The physics algorithm used in this system was very important to create the smooth projectile drop motion. I learnt the algorithm off a pretty imformant DevForum post and translated it into Luau. How it works: (If you're interested in the DevForum post, links wil be shown below!)
 ```
local distance = (bullet.oldposition.p - bullet.position.p).magnitude
-->>: Distance based on the old position and the new position. This is used later in the raycasting in order to find objects between the 2 points.
bul.CFrame = CFrame.new(bullet.oldposition.p, bullet.position.p) * CFrame.new(0,0, -distance*0.5)
-->>: Sets projectile CFrame to look at its new position to update its orientation. 
-->>: The distance is halved so the projectile can be placed between the 2 points (The old & new positions).
-->>: Projectile ends up being between the 2 points, to help us visualize how the ray casting in the game world.
local newray = Ray.new(bullet.oldposition.p, (bullet.position.p - bullet.oldposition.p).unit * distance)
-->>: Creates a ray pointing from the previous position to the new position with the distance defined.
local hit = workspace:FindPartOnRayWithIgnoreList(newray, {--Tables/Parts you want to ignore})
-->>: Create an ignore list to error check the raycast collisions. Normally you want to keep the players character and camera in here.
```


