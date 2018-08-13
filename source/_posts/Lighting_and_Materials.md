---
title: Lighting and Materials
date: 2018-07-09 20:59:05
thumbnail: http://hucoco.com/img/Graphics/RayTracing/lighting_and_matrials_00.png
mathjax: true
tags: 
- Graphics
- Ray Tracing
categories:
- Graphics
- Ray Tracing
---

## Lighting

Caculate lighting effect is extremely complicated and it depends on way too many factors in real world, therefore we use a method based on approximations of reality using simplified models that are much easier to process and look relatively similar. These lighting models are based on the physics of light as we understand it. One of those models is called the **Phong lighting model**. 

Phong lighting is a great and very efficient approximation of lighting, but its specular reflections break down in certain conditions, specifically when the shininess property is low resulting in a large (rough) specular area.

![](http://hucoco.com/img/Graphics/RayTracing/lighting_and_matrials_01.png)

<!--more-->

In 1977 the **Blinn-Phong lighting model** was introduced by James F. Blinn as an extension to the Phong shading we've used so far. The Blinn-Phong model is largely similar, but approaches the specular model slightly different which as a result overcomes our problem. Instead of relying on a reflection vector we're using a so called halfway vector that is a unit vector exactly halfway between the view direction and the light direction. The closer this halfway vector aligns with the surface's normal vector, the higher the specular contribution.

![](http://hucoco.com/img/Graphics/RayTracing/lighting_and_matrials_02.png)

Getting the halfway vector is easy, we add the light's direction vector and view vector together and normalize the result:

$$
\overline{H} = \frac{\overline{V} + \overline{H}}
{\begin{Vmatrix}
\overline{V} + \overline{H}
\end{Vmatrix}}
$$


## Matrials

The major building blocks of the Phong model consist of 3 components: ambient, diffuse and specular lighting. Below you can see what these lighting components actually look like:

![](http://hucoco.com/img/Graphics/RayTracing/lighting_and_matrials_00.png)

* **Ambient lighting:** even when it is dark there is usually still some light somewhere in the world (the moon, a distant light) so objects are almost never completely dark. To simulate this we use an ambient lighting constant that always gives the object some color.
* **Diffuse lighting:** simulates the directional impact a light object has on an object. This is the most visually significant component of the lighting model. The more a part of an object faces the light source, the brighter it becomes.

	```C++
	ptLight.I_source * mat->k_d * NL
	```
	
* **Specular lighting:** simulates the bright spot of a light that appears on shiny objects. Specular highlights are often more inclined to the color of the light than the color of the object.

	```C++
	float RVn = pow( (float) dot( V, R ), (float) mat->n);  \\Phong lighting model
	float RVn = pow( (float) dot( N, H ), (float) mat->n);  \\Blinn-Phong lighting model
	ptLight.I_source * mat->k_r * RVn);
	```

## Phong lighting model

```C++
static Color computePhongLighting( const Vector3d &L, const Vector3d &N, const Vector3d &V,
								   const EMPMaterial* mat, const PointLightSource &ptLight )
{
	Vector3d NN = ( dot( L, N ) >= 0.0 )?  N : -N;

	Vector3d R = mirrorReflect( L, NN );
	float NL = (float) dot( NN, L );
	float RVn = pow( (float) dot( V, R ), (float) mat->n );

	return ptLight.I_source * ( mat->k_d * NL  +  mat->k_r * RVn );
}
```

## Blinn-Phong lighting model

```C++
static Color computeBlinnPhongLighting(const Vector3d &L, const Vector3d &N, const Vector3d &V,
    const EMPMaterial* mat, const PointLightSource &ptLight)
{
    Vector3d NN = (dot(L, N) >= 0.0) ? N : -N;

    Vector3d R = mirrorReflect(L, NN);
    Vector3d H = L + V;
    H = H.makeUnitVector();
    float NL = (float)dot(NN, L);
    float RVn = pow((float)dot(N, H), (float)mat->n);

    return ptLight.I_source * (mat->k_d * NL + mat->k_r * RVn);
}

```

those lighting models are experience model, it is not base on physically visual effect. 

physically based rendering is a collection of render techniques, these techniques will be introduced in other series of articles. 

[Physically Based Rendering Series Catalogue](http://hucoco.com/2018/07/04/Physically-Based-Rendering-Catalogue/), and  the series is in progress.

## Reference

**[Learn OpenGL](https://learnopengl.com/) **
