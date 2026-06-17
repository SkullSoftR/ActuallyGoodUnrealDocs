## Writing classes in Unreal

## What is a class?

Object-oriented programming is a technique well suited to game development, which is why Unreal makes use of it. Essentially, a class can be thought of as a template or blueprint for a object that exists in the game. It contains all the logic that the object might need, which we can leverage to easily have many different objects in the game that do similar things - think different enemy types or items, etc.

A developer might write an _Enemy_ class, which has logic for patrolling around and following/attacking the player when they see them. They can then create many instances (also referred to as _objects_) of that enemy in the code, each with a different 3D model, voice line or walk speed.

In order for classes to do things, they will have both _properties_ (equivalent to variables in other programming languages) and _methods_ (functions). The properties are the data — for example the amount of health an enemy might have — and the methods contain the actual behaviour, like moving around and shooting etc.

In Unreal Engine specifically, every interactive thing in the game will first be written as a class in C++. When you drag an Enemy class from the Content Browser into the level, you are creating an _instance_ of that class.

## UCLASS() explained

## Unreal's Base classes
