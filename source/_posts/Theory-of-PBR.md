---
title: Theory of PBR
date: 2018-07-10 22:16:57
thumbnail: http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_00.png
tags: 
- Graphics
- PBR
categories:
- Graphics
- PBR
---

![](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_00.png)

## Overview

PBR (**Physically Base Rendering**) is a rendering technique that are more or less based on the same underlying theory which more closely matches that of the physical world. the biggest difference between the physical rendering model and the traditional rendering method is that it can more accurately describe and draw the interaction between the light and the surface of the object.

<!--more-->

## Diffusion and Reflection

Diffusion and Reflection are two words that describe the most basic differences in light and surface interaction.

When light hits the edge of the surface of the object, some of the light will reflect from the surface. On a smooth surface, which makes the surface looks like a mirror. **Specular Reflection** is often used to describe this effect.

But not all of light will reflect form the surface, Usually some light will penetrate into the inside of the object.

In this case, the light is either absorbed by the object (converted to heat) or scattered within the object. Some scattered light may return to the surface and be captured by the eye or camera. This is the well-known **Diffuse**.

By the way, Diffuse and Subsurface Scattering are all describe the same phenomenon.

Scattering is usually random in the direction, so **albedo** is a color that describes which color is most easily scattered back.

## Translucency and Transparency

In some cases, diffuse reflections become more complicated—in materials with longer scattering distances, such as skin, wax, jade, and so on.In this case a simple color usually does not determine everything, and it also must consider the shape and thickness of the object.If these objects are thin enough, these objects usually have some light scattered from one surface and the nemitted from the other side. This phenomenon is called Translucent. If the scattering intensity is lower (such as glass), so that the scattered part is almost negligible, then the landscape on the other side of the object can directly enter the human eye through the object. This phenomenon is very far from the typical "difficulty near the surface" of the diffuse reflection, so a special shader is usually needed to simulate them.

## Mircofacet Model

All the PBR techniques are based on the theory of microfacets. The theory describes that any surface at a microscopic scale can be described by tiny little perfectly reflective mirrors called microfacets. According to the roughness of a surface the alignment of these tiny little mirrors can differ quite a lot:

![](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_03.png)

The rougher a surface is, the more chaotically aligned each microfacet will be along the surface. The effect of these tiny-like mirror alignments is that when specifically talking about specular lighting/reflection the incoming light rays are more likely to scatter along completely different directions on rougher surfaces, resulting in a more widespread specular reflection. In contrast, on a smooth surface the light rays are more likely to reflect in roughly the same direction, giving us smaller and sharper reflections:

![](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_04.png)

## Energy Conservation

Energy Conservation is describe a phenomenon: outgoing light energy should never exceed the incoming light energy (excluding emissive surfaces). 

![roughness increase form left to right](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_02.png)

Looking at the above image we see the specular reflection area increase, but also its brightness decrease at increasing roughness levels. If the specular intensity were to be the same at each pixel regardless of the size of the specular shape the rougher surfaces would emit much more energy, violating the energy conservation principle. This is why we see specular reflections more intensely on smooth surfaces and more dimly on rough surfaces.

For energy conservation to hold we need to make a clear distinction between diffuse and specular light. The moment a light ray hits a surface, it gets split in both a refraction part and a reflection part. The reflection part is light that directly gets reflected and doesn't enter the surface; this is what we know as specular lighting. The refraction part is the remaining light that enters the surface and gets absorbed; this is what we know as diffuse lighting.

From physics, we know that light can effectively be considered a beam of energy that keeps moving forward until it loses all of its energy, the way a light beam loses energy is by collision. Each material consists of tiny little particles that can collide with the light ray as illustrated below. The particles absorb some or all of the light's energy at each collision which is converted into heat.

![](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_01.png)

## PBR materials

![](http://hucoco.com/img/Graphics/PBR/Threoy_of_PBR_00.png)

**Albedo:** the albedo texture specifies for each texel the color of the surface, or the base reflectivity if that texel is metallic. This is largely similar to what we've been using before as a diffuse texture, but all lighting information is extracted from the texture. Diffuse textures often have slight shadows or darkened crevices inside the image which is something you don't want in an albedo texture; it should only contain the color (or refracted absorption coefficients) of the surface.

**Normal:** the normal map texture is exactly as we've been using before in the normal mapping tutorial. The normal map allows us to specify per fragment a unique normal to give the illusion that a surface is bumpier than its flat counterpart.

**Metallic:** the metallic map specifies per texel whether a texel is either metallic or it isn't. Based on how the PBR engine is set up, artists can author metalness as either grayscale values or as binary black or white.

**Roughness:** the roughness map specifies how rough a surface is on a per texel basis. The sampled roughness value of the roughness influences the statistical microfacet orientations of the surface. A rougher surface gets wider and blurrier reflections, while a smooth surface gets focused and clear reflections. Some PBR engines expect a smoothness map instead of a roughness map which some artists find more intuitive, but these values get translated (1.0 - smoothness) to roughness the moment they're sampled.

**AO:** the ambient occlusion or AO map specifies an extra shadowing factor of the surface and potentially surrounding geometry. If we have a brick surface for instance, the albedo texture should have no shadowing information inside the brick's crevices. The AO map however does specify these darkened edges as it's more difficult for light to escape. Taking ambient occlusion in account at the end of the lighting stage can significantly boost the visual quality of your scene. The ambient occlusion map of a mesh/surface is either manually generated or pre-calculated in 3D modeling programs.

## Reference

**[Learn OpenGL PBR Theory](https://learnopengl.com/PBR/Theory) **

