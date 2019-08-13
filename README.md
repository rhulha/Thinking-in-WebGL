---
title: Thinking in WebGL
active_menu: learn
---

<iframe src="https://www.babylonjs.com/demos/csg/" width="100%" height="400" frameBorder="0"></iframe>

Writing an application in WebGL is a great choice - WebGL is a fantastic API!

But there are a couple of things you need to know to be **efficient with WebGL**, especially if you come from a desktop application background.

This article series will cover the most important things to do and keep in mind when developing a WebGL application using your own or a 3rd party engine. The goals are to achieve **high frame rates** and **small loading times**!

Topics are:

##General

[General Differences between WebGL and Native 3D Applications](../general/)

##Reducing Load Time

* [Reducing Code Size](../reducing-code-size/)
* [Reducing Texture Size](../reducing-texture-size/)
* [Procedurally Generated Geometry](../procedurally-generated-geometry/)
* [Procedural Textures](../procedural-textures/)

##Reducing Render Time

* [Reducing Memory Usage](../reducing-memory-usage/)
 * Object Creation and Garbage Collection
 * Polygon Reduction
* [Reducing Draw Calls](../reducing-draw-calls/)
 * MeshBuilder
 * EntityCombiner
* [Reducing CPU calculations](../reducing-cpu-calculations/)
 * Moving Calculations to the GPU