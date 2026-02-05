# Un-sloppifying Unreal's visuals

Unreal Engine 5, while powerful, unfortunately makes new developers feel completely out of control of how their game looks when they start a new project.
So many simple games are using insanely advanced features like real-time global illumination and ray tracing when they have no reason to whatsoever. I don't like how Epic has enabled unneccessary things like AI upscaling and temporal anti-aliasing by default, and it's very poorly explained online how you actually change it. So here's how to do it!

It's best to do as much of this as possible by writing in your project's _Config/DefaultEngine.ini_ file, but unfortunately some of it must still be done in the Unreal Editor.

## 1. Anti-aliasing

```
; Disable AA and TAA
r.AntiAliasingMethod=0
r.TemporalAA.Upsampling=0
r.TSR.ShadingRejection=0
r.TSR.History.GrandReprojection=0
r.TSR.Velocity.WeightClamping=0
```
Unreal 5's default temporal anti-aliasing leads to a lot of ghosting and unwanted visual noise. It can be disabled by setting _r.AntiAliasingMethod_ to **0**, or you can specify **1** to use FXAA which is a much better-looking alternative that still reduces jagged edges.

## 2. Lumen

```
r.DynamicGlobalIlluminationMethod=0
r.Lumen.GlobalIllumination=0
r.Lumen.Reflections=0
r.Lumen.ScreenProbeGather=0
```

Lumen is really nice in theory, but it uses a lot of temporal techniques and unfortunately in my opinion the technology just isn't mature enough to reach expectations.

## 3. Nanite

The same goes for Nanite, auto LODs are a really cool idea but for the vast majority of projects it's just unneccessary overhead.

With the Unreal Editor open, go to _Project Settings -> Rendering -> Nanite_ and make sure it is unchecked.
If you get a red error message in the editor later on, you need to make sure that _Project Settings -> Rendering -> Generate Nanite Fallback Meshes_ is checked.

You should then search for all the _Static Mesh_ files in your project, and then _right click -> Asset Actions... -> Edit Selection in Property Matrix_.
Once you have the Property Matrix open, un-collapse Nanite settings and make sure Enabled is completely unchecked.

## 4. Hardware ray tracing

Ray tracing is cool, but ridiculously overkill for small game-dev projects.

With the Unreal Editor open, go to _Project Settings -> Rendering_ and make sure that _Support Hardware Ray Tracing_ is disabled.

## 5. Motion Blur

```
r.DefaultFeature.MotionBlur=False
r.MotionBlur.Amount=0
r.DefaultFeature.MotionBlur=0
r.VelocityOutputPass=0
```

Motion blur is a sticking point for a lot of players, most people don't mind it too much if it's done well, but Unreal 5's implementation of it leads to a lot of ghosting, so we'll turn it off.

## 6. Temporal post-processing effects

```
r.AmbientOcclusionTemporalBlendWeight=0
r.SSR.Temporal=0
r.SSR.HalfResSceneColor=0
r.PostProcessAAQuality=2
```

These are left-over features from the upscaling that we're no longer using, so we'll disable them.

## 7. Virtual Textures, Texture Streaming and Virtual Shadow Maps

```
r.Shadow.Virtual.Enable=0
r.VirtualTextures=False
r.TextureStreaming=False
```

These features are pretty new and also very overkill for a small project. If you have 10,000 textures and lots of lighting in your game this might be worth considering but for most projects, spending computing time on this will not be worth it.

## 8. Ensure native resolution

```
r.Upscale.Quality=0
r.ScreenPercentage=100
r.SecondaryScreenPercntage.GameViewport=100
```

This feels really silly but in my experience has genuinely made a lot of difference. You can additionally set the percentages to **200**, which will 'supersample' the rendering. This means the game is being drawn at a really crisp 4K and then downscaled if the player's monitor isn't big enough, reducing jagged edges.

## 9. Consider forward rendering rather than deferred

This will be a much bigger consideration than the other options as it reduces the amount of lights you can have at one time, but it also allows for better anti-aliasing and (potentially) performance. Deferred rendering should be fine for the vast majority of projects, but you might want to read up on forward rendering to see if your game is simple enough to benefit from the performance gains.

If you want to use forward rendering, go to _Project Settings -> Rendering -> Forward Renderer_ and make sure _Forward Shading_ is checked.

## Bonus: getting a retro look

Set the screen percentages to 50 or 25 as you see fit.
Use the Property Matrix on all your textures and set Filtering -> Nearest and Mip Gen Settings -> No Mip Maps.

**TODO: consider Shader Compilation Stutter?**
