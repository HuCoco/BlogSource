---
title: Do Ray Tracing
date: 2018-07-10 13:54:10
thumbnail: http://hucoco.com/img/Graphics/RayTracing/DoRayTracing_01.png
mathjax: true
tags: 
- Graphics
- Ray Tracing
categories:
- Graphics
- Ray Tracing
---

![](http://hucoco.com/img/Graphics/RayTracing/DoRayTracing_01.png)

## Generate Ray

Generate a ray that goes from the camera's origin through the pixel location (pixelPosX, pixelPosY) of the camera. Note that pixelPosX and pixelPosY can be non-integer. The image origin is at the bottom-leftmost corner, that means:

* The bottom-leftmost corner of the image is (0, 0).
* The top-rightmost corner of the image is (imageWidth, imageHeight).
* The center of the bottom-leftmost pixel is (0.5, 0.5). 
* The center of the top-rightmost pixel is (imageWidth-0.5, imageHeight-0.5).

```c++
Ray getRay( double pixelPosX, double pixelPosY ) const
{
	Vector3d imgPos = mImageOrigin + (pixelPosX/mImageWidth) * mImageU + (pixelPosY/mImageHeight) * mImageV;
	return Ray( mCOP, imgPos - mCOP );
}
```

<!--more-->

## Nearest Surface

Find whether and where the ray hits some surface. Take the nearest hit point. 

```C++
bool hasHitSomething = false;
double nearest_t = DEFAULT_TMAX;
SurfaceHitRecord nearestHitRec;

for ( int i = 0; i < scene.numSurfaces; i++ )
{
	SurfaceHitRecord tempHitRec;
	bool hasHit = scene.surfacep[i]->hit( uRay, DEFAULT_TMIN, DEFAULT_TMAX, tempHitRec );

	if ( hasHit && tempHitRec.t < nearest_t )
	{
		hasHitSomething = true;
		nearest_t = tempHitRec.t;
		nearestHitRec = tempHitRec;
	}
}
```

## Shadow

Add to result the phong lightin contributed by each point light source. Compute shadow if hasShadow is true.


```c++
bool shadow = true;
for(int i = 0 ; i < scene.numPtLights ; i++)
{
    
    Vector3d Lin = scene.ptLight[i].position - nearestHitRec.p;
    double MaxLength = (Lin).length();
    double invLen = 1 / MaxLength;
    Vector3d L = (Lin)*invLen;

    if(hasShadow)
    {
        Ray newRay(nearestHitRec.p,L);
        for(int j = 0 ; j < scene.numSurfaces ; j++)
        {
            if(scene.surfacep[j]->shadowHit(newRay, DEFAULT_TMIN, MaxLength))
            {
                shadow = false;
                break;
            }
        }
        if(shadow == false)
        {
            continue;
        }
    }
    result += computePhongLighting(L, N, V, *nearestHitRec.mat_ptr, scene.ptLight[i]);
}
```

## Reflection

Add to result the reflection of the scene.

```C++
if(reflectLevels > 0)
{
    Vector3d dir = mirrorReflect(V, N);
    Ray rRay(nearestHitRec.p,dir);
    result += nearestHitRec.mat_ptr->k_rg * TraceRay(rRay,scene,--reflectLevels,hasShadow);
}
```

## Hammersley Sampling

We know that binary numbers are used to represent data in the computer. The corresponding relations between decimal and binary systems are shown in the following table. 

|Decimal|Binary|
|:-:|:-:|
|1|1|
|2|10|
|3|11|
|4|100|

And Hammersley Sampling is to generate uniformly distributed 2D random sampliong points by using this characteristics.

It constructs a value by implementing a **Radical Inverse** method for a binary number. Its process is as follows

|Decimal|Binary|Radical Inverse|Value|
|:-:|:-:|:-:|:-:|
|1|1|.1 = 1 * 1/2|0.5|
|2|10|.01 = 0 * 1/2 + 1 * 1/4|0.25|
|3|11|.11 = 1 * 1/2 + 1 * 1/4|0.75|
|4|100|.001 = 0 * 1/2 + 0 * 1/4 + 1 * 1/8|0.125|

Constructing a set of 2D random sampling points for Hammersley Sampling

$$\phi(i) = RadicalInverse(i)$$
$$P_i = (x_i,y_i) = (i/N,\phi(i))$$

## Anti-Aliasing

In ray tracing world, Anti-Aliasing method is simply, it only just increase the amount of sampling.

#### Anti-Aliasing in GLSL

```c
for(uint i = 0 ; i < NumSample; i++)
{
	vec2 offset = Hammersley(i, NumSample);
	Ray ray = getRay(float(pos.x) + offset.x, float(pos.y) + offset.y); // create ray from camera
	color += RayTrace(ray,ReflectLevels,HasShadow);
}
```

![Without Anti-Aliasing](http://hucoco.com/img/Graphics/RayTracing/DoRayTracing_00.png)

![With Anti-Aliasing](http://hucoco.com/img/Graphics/RayTracing/DoRayTracing_01.png)

## Code


```C++
for (uint32_t y = task->beginHeight; y < task->endHeight; y++)
{
    for (uint32_t x = 0; x < task->width; x++)
    {
        Color pixelColor = Color(0, 0, 0);
        for (uint32_t i = 0; i < task->numSample; i++)
        {
            float xx;
            float yy;
            Math::Hammersley(i, task->numSample, &xx, &yy);
            double pixelPosX = x + xx;
            double pixelPosY = y + yy;
            Ray ray = task->scene->camera.getRay(pixelPosX, pixelPosY);
            pixelColor += Raytrace::TraceRay(ray, *task->scene, task->reflectLevels, task->hasShadow);
        }
        pixelColor /= (float)task->numSample;
        pixelColor.clamp();
        task->output->setPixel(x, y, pixelColor);

    }
    // printf( "%d ", y );
}
```

```C++
Color Raytrace::TraceRay( const Ray &ray, const Scene &scene, 
					      int reflectLevels, bool hasShadow )
{
	Ray uRay( ray );
	uRay.makeUnitDirection();  // Normalize ray direction.

	bool hasHitSomething = false;
	double nearest_t = DEFAULT_TMAX;
	SurfaceHitRecord nearestHitRec;

	for ( int i = 0; i < scene.numSurfaces; i++ )
	{
		SurfaceHitRecord tempHitRec;
		bool hasHit = scene.surfacep[i]->hit( uRay, DEFAULT_TMIN, DEFAULT_TMAX, tempHitRec );

		if ( hasHit && tempHitRec.t < nearest_t )
		{
			hasHitSomething = true;
			nearest_t = tempHitRec.t;
			nearestHitRec = tempHitRec;
		}
	}

	if ( !hasHitSomething ) return scene.backgroundColor;

	nearestHitRec.normal.makeUnitVector();
	Vector3d N = nearestHitRec.normal;	// Unit vector.
	Vector3d V = -uRay.direction();		// Unit vector.
    
	Color result( 0.0f, 0.0f, 0.0f );	// The result will be accumulated here.

    bool shadow = 1.0f;
    for(int i = 0 ; i < scene.numPtLights ; i++)
    {
        
        Vector3d Lin = scene.ptLight[i].position - nearestHitRec.p;
        double MaxLength = (Lin).length();
        double invLen = 1 / MaxLength;
        Vector3d L = (Lin)*invLen;

        if(hasShadow)
        {
            Ray newRay(nearestHitRec.p,L);
            for(int j = 0 ; j < scene.numSurfaces ; j++)
            {
                if(scene.surfacep[j]->shadowHit(newRay, DEFAULT_TMIN, MaxLength))
                {
                    shadow = true;
                    break;
                }
            }
            if(shadow == false)
            {
                continue;
            }
        }
        result += computePhongLighting(L, N, V, *nearestHitRec.mat_ptr, scene.ptLight[i]);
    }
    
    result += scene.amLight.I_a * nearestHitRec.mat_ptr->k_a;

    if(reflectLevels > 0)
    {
        Vector3d dir = mirrorReflect(V, N);
        Ray rRay(nearestHitRec.p,dir);
        result += nearestHitRec.mat_ptr->k_rg * TraceRay(rRay,scene,--reflectLevels,hasShadow);
    }


	return result;
}
```

## Reference

**[Learn OpenGL](https://learnopengl.com/) **