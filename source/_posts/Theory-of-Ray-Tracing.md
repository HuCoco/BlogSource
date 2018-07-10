---
title: Ray Tracing Overview
date: 2018-07-05 20:09:50
thumbnail: http://hucoco.com/img/Graphics/RayTracing/RayTracing_1.png
tags: 
- Graphics
- Ray Tracing
categories:
- Graphics
- Ray Tracing
---

## Overview


![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_10.png)

In 3D compute graphics, ray tracing is rendering technique for generating an visual image by tracing the path of light. it has better effect than either ray casting and scan-line rendering techniques.

Typically, there are two question need be deal:

* What object has been seen?
* What color of the object is under the influence of light and environment?

In nature, a ray will travel to a surface that stop ray traveling, or until out of energy eventually disappears.
there are four things might happen with light ray: **absorption**, **reflection**, **refraction**, **fluorescence**,

<!--more-->

### Ray Casting

In ancient time, it was used for the study of perspective.

![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_2.png)

The first ray tracing algorithm used for rendering was presented by Arthur Appel in 1968.[1] This algorithm has since been termed "ray casting". The idea behind ray casting is to shoot rays from the eye, one per pixel, and find the closest object blocking the path of that ray. Think of an image as a screen-door, with each square in the screen being a pixel. This is then the object the eye sees through that pixel. Using the material properties and the effect of the lights in the scene, this algorithm can determine the shading of this object. The simplifying assumption is made that if a surface faces a light, the light will reach that surface and not be blocked or in shadow. The shading of the surface is computed using traditional 3D computer graphics shading models. One important advantage ray casting offered over older scanline algorithms was its ability to easily deal with non-planar surfaces and solids, such as cones and spheres. If a mathematical surface can be intersected by a ray, it can be rendered using ray casting. Elaborate objects can be created by using solid modeling techniques and easily rendered.

![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_3.png)
![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_4.png)

### Ray Tracing

it almost like ray casting, but in ray tracing, it will generate some new ray from the closet intersection point.

* Reflection ray
* Refraction ray
* Shadow rays

![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_5.png)

and this technique also called Recursive Ray Tracing.

#### Advantages

Ray tracing's popularity stems from its basis in a realistic simulation of lighting over other rendering methods (such as scanline rendering or ray casting). Effects such as reflections and shadows, which are difficult to simulate using other algorithms, are a natural result of the ray tracing algorithm. The computational independence of each ray makes ray tracing amenable to parallelization.

#### Disadvantages

A serious disadvantage of ray tracing is performance (though it can in theory be faster than traditional scanline rendering depending on scene complexity vs. number of pixels on-screen). Scanline algorithms and other algorithms use data coherence to share computations between pixels, while ray tracing normally starts the process anew, treating each eye ray separately. However, this separation offers other advantages, such as the ability to shoot more rays as needed to perform spatial anti-aliasing and improve image quality where needed.

Although it does handle interreflection and optical effects such as refraction accurately, traditional ray tracing is also not necessarily photorealistic. True photorealism occurs when the rendering equation is closely approximated or fully implemented. Implementing the rendering equation gives true photorealism, as the equation describes every physical effect of light flow. However, this is usually infeasible given the computing resources required.

The realism of all rendering methods can be evaluated as an approximation to the equation. Ray tracing, if it is limited to Whitted's algorithm, is not necessarily the most realistic. Methods that trace rays, but include additional techniques (photon mapping, path tracing), give far more accurate simulation of real-world lighting.

#### Ray Tracing Detail

![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_6.png)
![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_7.png)
![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_8.png)

#### Shadow

At each surface intersection point,a shadow ray is shot towards eachlight source to determine any occlusion between light source and surface point.

![](http://hucoco.com/img/Graphics/RayTracing/RayTracing_9.png)

---

## Reference

**[Ray Tracing Wiki](https://en.wikipedia.org/wiki/Ray_tracing) **

**[NUSRI Summer Programme 2016, 3D Graphics Rendering, Lecture 9 Ray Tracing, School of Computing National University of Singapore](http://hucoco.com/file/lec09_ray_tracing.pdf)**