---
title: Ray and Camera 
date: 2018-07-07 16:08:16
thumbnail: http://hucoco.com/img/Graphics/RayTracing/ray_00.png
mathjax: true
tags: 
- Graphics
- Ray Tracing
categories:
- Graphics
- Ray Tracing
---

## Ray

Finding ray-object  intersection and computing  surface normal is central to ray tracing.

Ray representations:

* Two 3D vectors
	* Ray origin position
	* Ray direction vector
* Parametric form
	* $P(t) = origin + t \times direction$

![](http://hucoco.com/img/Graphics/RayTracing/ray_00.png)

<!--more-->

### Computing Reflection / Refraction Rays

![](http://hucoco.com/img/Graphics/RayTracing/ray_01.png)

#### Snell’s law

$$\mu_1sin\theta = \mu_2sin\phi$$

#### Reflection

$$R = 2 (N \cdot L) N - L$$

#### Refraction

$$\mu = \frac{\mu_1}{\mu_2}$$

$$T = -\mu L + (\mu (N \cdot L) - \sqrt{1 - \mu^2 (1 - (N \cdot L)^2)})N$$

### Recursive Ray Tracing

For each reflection/refraction ray spawned, we can trace it just like tracing the original ray.

![](http://hucoco.com/img/Graphics/RayTracing/ray_02.png)

#### When to stop recursion?

* When the surface is totally diffuse (and opaque)
* When reflected/refracted ray hits nothing
* When maximum recursion depth is reached
* When the contribution of the reflected/refracted ray to thecolor at the top level is too small
	* $(K_{rg1} | K_{tg1}) \times \cdots \times (k_{rg(n-1)}|k_{tg(n-1)}) < threshold $

## Camera

Camera view & image resolution

* Camera position and orientation in world coordinate frame
	* Similar to gluLookAt()
* Field of view
	* Similar to gluPerspective(), but no need near & far plane
* Image resolution
	* Number of pixels in each dimension

```c++
Camera &Camera::setCamera( 
			    const Vector3d &eye, const Vector3d &lookAt, const Vector3d &upVector,
		        double left, double right, double bottom, double top, double near,
			    int image_width, int image_height )
{
	assert( image_width > 0 && image_height > 0 );
	mImageWidth = image_width;
	mImageHeight = image_height;

	mCOP = eye;
	Vector3d cop_n = (eye - lookAt).unitVector();
	Vector3d cop_u = cross( upVector.unitVector(), cop_n );
	Vector3d cop_v = cross( cop_n, cop_u );

	mImageOrigin = mCOP + ( left * cop_u ) + ( bottom * cop_v ) + ( -near * cop_n );

	mImageU = (right - left) * cop_u;
	mImageV = (top - bottom) * cop_v;
	return (*this);
}

Ray getRay( double pixelPosX, double pixelPosY ) const
{
	Vector3d imgPos = mImageOrigin + (pixelPosX/mImageWidth) * mImageU + (pixelPosY/mImageHeight) * mImageV;
	return Ray( mCOP, imgPos - mCOP );
}

```

## Reference

**[Learn OpenGL](https://learnopengl.com/) **