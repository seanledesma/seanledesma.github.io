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

Going to post the code here for now, will return later to finish this post

```c
#include <raylib.h>

#include <math.h>

#include <stdlib.h>

  

#define screenWidth 800

#define screenHeight 450

#define ballCount sizeof(balls) / sizeof(balls[0])

  

// this uses both typedef and struct tag :)

typedef struct Ball{

Vector2 position;

Vector2 speed;

float radius;

} Ball;

  
  
  
  

int main(void) {

InitWindow(screenWidth, screenHeight, "particle physics");

  

Ball balls[10] = {};

  

for(int i = 0; i < ballCount; i++) {

balls[i].position = (Vector2) { GetRandomValue(20, screenWidth), GetRandomValue(20, screenHeight) };

balls[i].speed = (Vector2) { 10.0f, 10.0f };

balls[i].radius = 20.0f;

}

  
  

SetTargetFPS(60);

//main game loop here

while (!WindowShouldClose()) {

for (int i = 0; i < ballCount; i++) {

// Move ball

balls[i].position.x += balls[i].speed.x;

balls[i].position.y += balls[i].speed.y;

  

// Wall collisions with position correction

if (balls[i].position.x >= (screenWidth - balls[i].radius)) {

balls[i].position.x = screenWidth - balls[i].radius; // Prevent sticking on the right

balls[i].speed.x *= -1.0f;

} else if (balls[i].position.x <= balls[i].radius) {

balls[i].position.x = balls[i].radius; // Prevent sticking on the left

balls[i].speed.x *= -1.0f;

}

  

if (balls[i].position.y >= (screenHeight - balls[i].radius)) {

balls[i].position.y = screenHeight - balls[i].radius; // Prevent sticking on the bottom

balls[i].speed.y *= -1.0f;

} else if (balls[i].position.y <= balls[i].radius) {

balls[i].position.y = balls[i].radius; // Prevent sticking on the top

balls[i].speed.y *= -1.0f;

}

  
  

// Ball-ball collisions

for (int j = i + 1; j < ballCount; j++) {

float dx = balls[j].position.x - balls[i].position.x;

float dy = balls[j].position.y - balls[i].position.y;

float distanceSquared = dx * dx + dy * dy;

float radiusSum = balls[i].radius + balls[j].radius;

  

if (distanceSquared <= radiusSum * radiusSum) {

// Calculate normal

Vector2 normal = { dx, dy };

float distance = sqrtf(distanceSquared);

normal.x /= distance;

normal.y /= distance;

  

// Calculate relative velocity

Vector2 relativeVelocity = { balls[j].speed.x - balls[i].speed.x, balls[j].speed.y - balls[i].speed.y };

  

// Calculate the velocity along the normal

float velocityAlongNormal = relativeVelocity.x * normal.x + relativeVelocity.y * normal.y;

  

// If the balls are moving away from each other, don't do anything

if (velocityAlongNormal > 0)

continue;

  

// Swap velocities along the normal direction

float restitution = 0.8f; // 1.0 for elastic collision (you can tweak it for less elastic collisions)

float impulseMagnitude = -(1 + restitution) * velocityAlongNormal;

  

// Apply impulse to the balls

balls[i].speed.x -= impulseMagnitude * normal.x / 2;

balls[i].speed.y -= impulseMagnitude * normal.y / 2;

balls[j].speed.x += impulseMagnitude * normal.x / 2;

balls[j].speed.y += impulseMagnitude * normal.y / 2;

  

// Push balls apart slightly to avoid sticking or repeated collisions

float overlap = radiusSum - distance;

balls[i].position.x -= normal.x * (overlap / 2);

balls[i].position.y -= normal.y * (overlap / 2);

balls[j].position.x += normal.x * (overlap / 2);

balls[j].position.y += normal.y * (overlap / 2);

}

}

  

}

  

BeginDrawing();

ClearBackground(BLACK);

  

//DrawCircleV(ballPosition, (float) ballRadius, WHITE);

for(int i = 0; i < (sizeof(balls) / sizeof(balls[0])); i++) {

DrawCircleV(balls[i].position, (float) balls[i].radius, WHITE);

}

  

DrawFPS(10, 10);

  

EndDrawing();

}

  

CloseWindow();

  

return 0;

}
```


