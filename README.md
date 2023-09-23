# Zeta Resources

* ### [About](https://github.com/Salamence064/Zeta-Resources#about-1)
* ### [The Zeta Codebase](https://github.com/Salamence064/Zeta-Resources#the-zeta-codebase-1)
* ### [Math for Zeta](https://github.com/Salamence064/Zeta-Resources#math-for-zeta-1)
* ### [Physics for Zeta](https://github.com/Salamence064/Zeta-Resources#physics-for-zeta-1)
* ### [Getting Started with Git and Github](https://github.com/Salamence064/Zeta-Resources#getting-started-with-git-and-github-1)
* ### [Contributors](https://github.com/Salamence064/Zeta-Resources#contributors-1)

___

## About
This document is meant to provide all the information someone new to Zeta would need to get started working with the engine. There are basics for math techniques, physics concepts, getting started with git and github, and a general introduction to working with our codebase. You do not need to read all of this but consider it a helpful resource for when you are unsure of how something in the engine works. If you have any questions reach out to Thomas or Utsawb (the PMs) on discord.

___

## The Zeta Codebase

* ##### [General Structure](https://github.com/Salamence064/Zeta-Resources#general-structure-1)
* ##### [ZMath](https://github.com/Salamence064/Zeta-Resources#zmath-1)
* ##### [Primitives](https://github.com/Salamence064/Zeta-Resources#primitives-1)
* ##### [Rigid and Static Bodies](https://github.com/Salamence064/Zeta-Resources#rigid-and-static-bodies-1)
* ##### [The Physics Handler](https://github.com/Salamence064/Zeta-Resources#the-physics-handler-1)
* ##### [Important Notes](https://github.com/Salamence064/Zeta-Resources#important-notes-1)
* ##### [2D Engine to Reference](https://github.com/Salamence064/Zeta-Resources#2d-engine-to-reference-1)

___

#### General Structure
* **6 Main Sections:**
  1. zmath.h
  2. primitives.h
  3. bodies.h
  4. intersections.h
  5. collisions.h
  6. physicshandler.h
<br>

* **Two Namespaces:**
  1. ZMath - contains all the math library code
  2. Zeta - contains all the physics related code
<br>

* **Coordinate System Used**
  * The origin is in the bottom left of the screen by default for Zeta
  * The position of primitives and rigid and static bodies is considered to be the centerpoint instead of the top left corner
  * Zeta will still work for any arbitrary coordinate system if the user provides a custom gravity value

Our engine works by accepting rigid and static bodies from the user in a physics handler. This physics handler then updates the positions, velocities, and net forces of those objects each frame for the user as depicted below. A codeblock example of a base implementation is also shown.

<br>

![](CodeStructure.jpg)

<br>

```c++
#include <ZETA/physicshandler.h>

int main() {
    // Create a physics handler with the default settings
    Zeta::Handler handler;

    // Create the colliders
    Zeta::Sphere* s1 = new Zeta::Sphere(ZMath::Vec3D(100.0f, 120.0f, 100.0f), 50.0f);
    Zeta::Sphere* s2 = new Zeta::Sphere(ZMath::Vec3D(350.0f, 500.0f, -340.0f), 200.0f);

    // Create some rigid bodies
    Zeta::RigidBody3D rb1(
        s1->c,                       // centerpoint
        50.0f,                       // mass
        0.9f,                        // coefficient of restitution
        0.975f,                      // linear damping
        Zeta::RIGID_SPHERE_COLLIDER, // collider type
        s1                           // collider
    );

    Zeta::RigidBody3D rb2(
        s2->c,                       // centerpoint
        100.0f,                      // mass
        0.95f,                       // coefficient of restitution
        0.8f,                        // linear damping
        Zeta::RIGID_SPHERE_COLLIDER, // collider type
        s2                           // collider
    );

    // Add the rigid bodies to the handler
    handler.addRigidBody(&rb1);
    handler.addRigidBody(&rb2);

    // Program's dt loop
    float dt = 0.0f;

    // Note: windowShouldNotClose would be replaced with the exit window condition in the user's graphics library
    while (windowShouldNotClose) {
        /* Rendering/Drawing code would go here */

        handler.update(dt); // The handler subtracts from dt for you
        // Note: getEllapsedTime() would be replaced with the equivalent in the user's graphics library
        dt += getEllapsedTime();
    }

    return 0;
};
```
___

#### ZMath

* ZMath is the math library created for use with Zeta
* There are 5 main components of ZMath:
  1. Vec2D
      * Models a 2D vector
      * Used to force a constant z-value with planes
  2. Vec3D
     * Models a 3D vector
     * Used in almost every part of Zeta
     * Represents positions, distances, forces, and more
  3. Mat2D
     * Models a 2x2 matrix
     * Is not used in the codebase, but useful to know it exists if needed
  4. Mat3D
     * Models a 3x3 matrix
     * Mainly used to store rotation matrices
  5. Math Utils
     * compare - compares equality of two floats or vectors within a given tolerance
     * clamp - force a value or a vector between a min and a max
     * abs - take the absolute value of our custom classes
     * min - return the minimum of two values
     * max - return the maximum of two values
     * toRadians - convert an angle from degrees to radians
     * rotateXY - rotate a point with respect to the XY plane about an origin
     * rotateXZ - rotate a point with respect to the XZ plane about an origin
<br>

* Handling Floating Point Error
  * Floats are imprecise
  * We handle this with a "tolerance"/"ɛ" value
  * If a value is within this tolerance/ɛ of another value, we consider them close enough to be equal

___

#### Primitives
* Often we can represent simple objects as primitives in a physics engine
* We can optimize collisions and computations for these specific types as we already know the type going in
* There are 6 primitives supported by Zeta:
  1. Ray3D
  2. Line3D
  3. Plane
  4. Sphere
  5. AABB (Unrotated Rectangular Prism)
  6. Cube (Rotated Rectangular Prism)
___

#### Rigid and Static Bodies

___

#### The Physics Handler

___

#### Important Notes

___

#### 2D Engine to Reference

* As a reference, check out [Zeta2D](https://github.com/Salamence064/Zeta2D)
* Zeta2D's a 2D version of our engine that I made
* 2D examples can help for visualizing or converting it to 3D
* Zeta2D does not have rotational kinematics yet
___

## Math for Zeta

___

## Physics for Zeta

___

## Getting Started with Git and Github

___

## Contributors
 * Thomas Ducote
 * Utsawb Lamichhane

