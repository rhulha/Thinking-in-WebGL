---
title: Procedurally Generated Geometry
active_menu: learn
---

A great way to reduce load times is to use procedurally generated geometry. There are many ways to generate geometry. Let’s go over the most important ones:

## Direct

The most basic approach is to generate the vertices directly. Here is a simple example generating the data for a 3D [cube](https://github.com/evanw/csg.js/blob/master/csg.js#L173).

The resulting data includes the vertices, the normals and the texture coordinates.

	function pushVertices( obj, v, n) {
	  for ( var i = 0; i<v.length; i++) {
		obj.v.push( 2*!(v[i]&1)-1, 2*!(v[i]&2)-1, 2*!(v[i]&4)-1 ); // vertices
		obj.n.push.apply( obj.n, n); // normals
	  }
	  obj.t.push( 0,0,1,0,1,1,0,1 ); // uv mapping
	}

	function generateCube() {
	  var obj = { v : [], n : [], t : [], i : []};
	  var van = [ // vertices and normals
		[[0, 4, 6, 2], [-1, 0, 0]],
		[[1, 3, 7, 5], [+1, 0, 0]],
		[[0, 1, 5, 4], [0, -1, 0]],
		[[2, 6, 7, 3], [0, +1, 0]],
		[[0, 2, 3, 1], [0, 0, -1]],
		[[4, 5, 7, 6], [0, 0, +1]]
	  ];
	  for ( var i = 0; i<van.length; i++) {
		pushVertices( obj, van[i][0], van[i][1] );
		obj.i.push( 4*i+0, 4*i+1, 4*i+2, 4*i+0, 4*i+2, 4*i+3);
	  }
	  return obj;
	}

Similar code can be used to generate [spheres](https://github.com/evanw/csg.js/blob/master/csg.js#L210) and [cylinders](https://github.com/evanw/csg.js/blob/master/csg.js#L252) and many other basic shapes.

## Surface of Revolution (or Lathe)

The [surface of revolution](http://en.wikipedia.org/wiki/Surface_of_revolution) technique uses a line or a spline curve and rotates it around an axis, for example the vertical axis. This technique is also known as lathing.

![](/img/learn/articles/thinking-in-webgl/proc_geom_0.png)

Here is an example using Goo Engine: [http://goote.ch/b3ddad02a95e44eb7d85e6f99540650dc9aa6def/](http://goote.ch/b3ddad02a95e44eb7d85e6f99540650dc9aa6def/)

The relevant code is this:

	var section = goo.PolyLine.fromCubicSpline([
	  3 + 0, -1, 0,
	  3 + 1,  0, 0,
	  3 + 1,  1, 0,
	  3 + 0,  1, 0,
	  3 - 1,  1, 0,
	  3 - 1,  2, 0,
	  3 + 0,  3, 0
	  ], 20);
	var latheMeshData = section.lathe(20);
	var material = new goo.Material(goo.ShaderLib.simpleLit);
	material.cullState.enabled = false;
	ctx.world.createEntity(latheMeshData, material).addToWorld();

Note: Because we can see the inside of the geometry the code turns of back face culling.

## Heightmap

Another great option especially for terrain is to use a heightmap to generate geometry. The basic task of a 3D heightmap function is to convert a map of values to an area of triangles. Here is an [example](http://code.gooengine.com/latest/visual-test/goo/geometrypack/Surface/HeightMap-vtest.html) using Goo Engine:

The relevant code for this is:

	function getHeightMap(nLin, nCol) {
		var matrix = [];
		for (var i = 0; i < nLin; i++) {
			matrix.push([]);
			for (var j = 0; j < nCol; j++) {
				matrix[i].push(Math.sin(i * 0.3) + Math.cos(j * 0.3)
				+ Math.sin(Math.sqrt(i*i + j*j) * 0.7) * 2);
			}
		}
		return matrix;
	}
	var matrix = getHeightMap(64,64);
	var meshData = Surface.createFromHeightMap(matrix);
	var material = new Material(ShaderLib.simpleLit);
	world.createEntity(meshData, material).addToWorld();

In this example the heightmap is generated using sine and cosine values. But you can also use gray scale images as input.

## Constructive Solid Geometry (CSG)

[CSG](http://en.wikipedia.org/wiki/Constructive_solid_geometry) is a process where one can use shapes and combine them together or subtract them from one another. The basic operations are **union**, **subtract**, and **intersect**.

A great option for a library that provides CSG is [CSG.js](http://evanw.github.io/csg.js/) from Evan Wallace.

![](/img/learn/articles/thinking-in-webgl/proc_geom_1.png)

## Extrusion

Extrusion is another great technique to generate 3D geometry from basic shapes like splines.

![](/img/learn/articles/thinking-in-webgl/proc_geom_2.gif)

A great demo of this technique can be found at the great site [acko.net](http://acko.net)

![](/img/learn/articles/thinking-in-webgl/proc_geom_3.png)

Just note how fast the site loads and displays 3D content. This is a perfect example on how to use WebGL efficiently.

# Further Reading

There are many more ways to procedurally generate 3D content, like fractals and NURBS and [meta balls](http://www.clicktorelease.com/code/bumpy-metaballs/) just to name a few. Here are some links to learn more:

[Polygonal Modeling](http://en.wikipedia.org/wiki/Polygonal_modeling)<br />
[Procedural Generation](http://en.wikipedia.org/wiki/Procedural_generation#Video_games)<br />
[GPU Gems 3 ch01](http://http.developer.nvidia.com/GPUGems3/gpugems3_ch01.html)<br />
[How To Do a Procedural City in 100 Lines](http://learningthreejs.com/blog/2013/08/02/how-to-do-a-procedural-city-in-100lines/)<br />

# Third Party Libraries That Implement Many Methods to Generate Geometry

[http://haptic-data.com/toxiclibsjs/](http://haptic-data.com/toxiclibsjs/)<br />
[http://verbnurbs.com/](http://verbnurbs.com/)<br />
[https://github.com/pboyer/verb](https://github.com/pboyer/verb)<br />

Next: [Procedural Textures](../procedural-textures/)

