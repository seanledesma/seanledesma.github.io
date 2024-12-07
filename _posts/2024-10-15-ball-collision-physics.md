---
layout: post
title: Ball Collision Physics in Raylib
---
I've been messing around with Raylib a bit more recently, trying to get circles to move around the screen and collide with each other. This seemed pretty trivial at first, but the difficulty of getting accurate collisions caught me by surprise.

I started by creating an array of ball structs, with each struct only containing three properties; position, speed and radius.
```c
typedef struct Ball{
    Vector2 position;
    Vector2 speed;
    float radius;
} Ball;
```

Inside the game loop, I would loop through the array of balls to update their position with their speed. So if the ball was going to the right of the screen, I'd add the ball's speed to the position, like so:
```c
// continue ball in direction at set speed
balls[i].position.x += balls[i].speed.x;
balls[i].position.y += balls[i].speed.y;
```

and if the ball hit a wall, I'd multiply the balls speed by negative one, so it would start subtracting from the balls position every loop instead of adding:
```c
// if balls hit wall, deflect right angle (?)
if (balls[i].position.x >= (screenWidth - balls[i].radius) || balls[i].position.x <= balls[i].radius)
    balls[i].speed.x *= -1.0f;
    
if (balls[i].position.y >= (screenHeight - balls[i].radius) || balls[i].position.y <= balls[i].radius)
    balls[i].speed.y *= -1.0f;
```

This works decently well, although it doesn't look natural at all. What ends up happening is the balls hit the wall, both the x and y speed gets multiplied by negative one, so it effectively makes the ball bounce off at a perfect 90 degree angle every time.

I then wanted to make the balls bounce off of each other, which seemed simple enough. You can detect collisions in Raylib between two circles like so:

```c
CheckCollisionCircles(balls[i].position, balls[i].radius, balls[j].position, balls[j].radius)
```

but this had some unexpected results. Upon collision, the balls would immediately flip directions and head back the way they came, which is obviously very unnatural. How on earth do I get the balls to bounce off each other in a natural-looking way?
