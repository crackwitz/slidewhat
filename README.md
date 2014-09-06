slidewhat
=========

Goal: identify the currently projected slide from ambient light.

## Assumptions

The camera maps scene radiance to pixel values. This likely involves a gamma-mapping.

The camera is panned and tilted through the scene.

The camera exhibits motion blur. Motion blur may be severe enough that only pictures taken at rest give usable readings.

The camera performs auto-exposure to keep pixel values within the representable range of the color space of the recording.

The camera applies white balance correction to the picture. This correction may be adjusted manually or automatically at any time, even continuously.

For radiometric calibration, a full white and full black picture may be projected. This is however not needed.

Primary light sources (ceiling lights, windows) are static in intensity or low-frequency. Light switching, flickering lights, or moving clouds will not be considered.

## Approach

Video projectors are significant light sources within a lecture hall or meeting room. Indirect light from a video projector gives clues as to what is currently being projected. This indirect light can be captured from the wall directly surrounding the projection, but also other objects in the scene that catch the light scattered from the screen or wall. Some static objects may even reflect a part of the projection specularly towards the camera.

I expect objects moving in relation to the projector and screen to be very much harder to get stable measurements from. Thus I will track only static objects in the scene, which I call "background". Objects that are static to one another have the property that they move uniformly through the camera picture. Of such moving groups, the largest group by area is taken to be background.

Parts of the background need to be recognized across frames. I will choose a suitable feature extractor and feature descriptor. Optical flow analysis may support this tracking effort.

We need to know if pixel values changed because the projection changed (secondary light source), or because either the primary light sources changed intensity or the camera adjusted its properties (exposure, white balance). Parts of the background that are relatively unaffected by the secondary (stable) will serve to gauge the primaries and determine current camera exposure and white balance properties.

Conceptually, pixel value change events can be attributed to camera property changes (exposure, white balance) or change of the secondary. Camera property changes affect the whole camera image uniformly and usually slowly, so as not to be noticeable to viewers. Intensity changes in the secondary are usually abrupt (hard slide transition) and affect some parts of the camera image more than others.

Once stable and affected regions of background are identified, the contribution of the secondary can be gauged and its changes analyzed. I will start by plotting this value and its first time derivative. Thresholding should identify interesting points in time and a corresponding analysis of the brightness and colors in the assumed slide set can be correlated with the camera video.
