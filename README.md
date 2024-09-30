# Split Render Tool 4 - Blender Addon
### Animated border region, Object based and Splits based renders

Are you getting "System is out of GPU memory" error? Do you want/need to render a 32K image? Did you render a sequence in which one object is wrong, but you don't want to render the whole sequence again? Do you have some older graphics cards (2GB, 4GB, etc) that can't process any of your projects? Split Render Tool is the add-on you need.

Supports Blender 4.1+, both Cycles and Eevee engines, and both OSX and Windows. (Blender versions below 4.1 as well as Linux systems had not been tested)

SRT4 brings users the ability to get rid of "System is out of GPU memory" and render at 200% of your resolution by rendering split tiles. On the other hand, it allows users to animate Blender's render region area frame by frame along the timeline. It also brings the ability to render based on selected object/s region on screen, by analizing their location and scale on each frame.

SRT goes far away from other similar add-ons, being able to render single frames as well as animations, and render higher resolutions with same hardware.

[Render Test: Blender max resolution: 900 x 500, SRT max resolution: 3840 × 2160](https://github.com/OlyDJ/SplitRenderTool4/wiki#example)

## Functionalities:

- [__Split Render__](https://github.com/OlyDJ/SplitRenderTool4/wiki#split-render): Split render screen into tiles and merge them without any user interaction (single frame or animation) in Compositior Editor. It creates a new Blender file, with all that splits already merged inside a group node (one group for each enabled View Layer), with all its passes (similar to an Image node with a multilayer file), ready to start your own composition.

- [__Animated Border Render__](https://github.com/OlyDJ/SplitRenderTool4/wiki#animated-border-render): Create Border Region keyframes along the timeline, and create a new Blender file with one Composition group node for each enabled View Layer (with all its passes), ready to add the original image sequence. 

- [__Object Based Render__](https://github.com/OlyDJ/SplitRenderTool4/wiki#objbect-based-render): Render selected object/s region on screen, and create a new Blender file with one Composition group node for each enabled View Layer (with all its passes), ready to add the original image sequence. 

Split Render Tool 4 is the next generation of Split Render Tool add-on. It had been re-coded from scratch taking the initial ideas, but re-thinking its methods to be more efficient in terms of render times and resources consumption. 


## Features:

### Split Render

It is well known that split a render into several pieces and merge them later has issues with Denoising internal AI. Blender Denoising works by analising all the image, processing all pixels, and returning the final image. So when we split an image, it just analise those pixels, but AI does not know what is beyond them. The resulting image after merge all pieces will have at the borders some pixels that will not match with the rest of the image. Leaving us with a useless image.

SRT fixes that issue WITHOUT user interaction, using 2 different methods:

- **Denoise Node**: Fix splits by using Denoising passes and Denoise node (Recommended).
- **Border Render**: Fix splits by rendering extra images (Old method, slow, requires more disk space).

Options:

- **Splits**: Select number of splits (6, 12, 24, 48, 96, 192 or 384).
- **Copy command line to clipboard**: Just open Terminal or CMD, paste and press enter. This way you can see the progress and all the processes.
- **Enable Cycles debug**: You will see more information about each render step.
- **Open Terminal/CMD**: Automatically open Terminal/CMD after copying CLI render command and closes Blender.
- **Use SRT Composition nodes**: Use SRT nodes directly to start compositing, otherwise render OpenEXR Multilayer images (one for each View Layer).
- **Mute Denoise nodes**: Only with "Denoise Node" method and "Use SRT Composition nodes". Useful at high resolutions to speed up Blender interface.
- **Auto disable Use Nodes in Composition Editor**: Only with "Denoise Node" method and "Use SRT Composition nodes". Useful at higher resolutions to speed up Blender interface.
- **Separate passes**: Only with "Use SRT Composition nodes" disabled. Separate each pass into files, instead of creating one multilayer file. 
- **Passes to folders**: Only with "Use SRT Composition nodes" disabled and "Separate passes" enabled. This option will create a folder for each View Layer, and inside of them a folder for each pass.
- **View Layers to folders**: Only with "Use SRT Composition nodes" disabled, "Separate passes" disabled, and at least 2 enabled View Layers. With this option SRT will create a folder for each View Layer.
- **Don't delete split images**: Only with "Use SRT Composition nodes" disabled. By default, after render final images, SRT deletes all splits and borders automatically. 
- **Use Blender's output**: By default SRT creates a directory where your .blend file is and with the same name as output for render. Enabled, this option will take Blender's output instead. 
- **Overwrite existing files**: This applies only to splits and borders, not to final images in any case. When disabled and if render crashes, init again SRT render, it will check and skip from render each existing split.

### Animated Border feature

This feature will save you a lot of render time. After a render, it can happen that some object are wrong, or you forgot to hide something, or just want to change some texture. You can create Border Region keyframes along the timeline (just like any other property). First you need to create keyframes using the in-built Keyframe System, then you must bake the animation (to process the rest of frames).

- Animate border region like any other object.
- Bake system to animate border. Create, Remove, Jump to First, Jump to previous, Jump to next, Jump to last, Bake animation, Reset bake and Reset keyframes buttons.
- Preview Render Region while scrolling the timeline, like animating any object property (after bake).
- **Create final composition**: One group node for each enabled View Layer (with all its passes), ready to add the original image sequence.
- **Use Blender's output**: By default SRT creates a directory where your .blend file is and with the same name as output for render. Enabled, this option will take Blender's output instead. 
- **Use ellipse mask**: By default SRT uses square mask.
- **Fade amount**: Select amount of blur edges size.

### Common features

- Option to use Blender's output file instead of the same folder as Blend file.
- Information about resolution and border sizes.
- Information about render times (Splits of a frame and frames).
- All image file formats.



"Use Blender's output". By default SRT creates a directory where your .blend file is, and with the same name. Split images have always same name: "srt_0005_0210.exr", where 0005 is the split number and 0210 is the frame number. 



SRT is compatible with Blender 2.93.3 to 3.6

Because of the way SRT works (at least yet), there is no preview while rendering, and Blender UI gets locked. Unfortunately to cancel the render 
process you need to kill blender app manually. (Something I'm trying hard to solve)

GPUs with 512, 1GB and 2GB might not be able to render big resolutions (8K +). This limitation will be removed in the future. 

By default the output is set to a new folder in Blender' s file folder, called as the Blend file. And the images names start with "srt_" 
followed by frame number (ex: srt_0012.xxx)

##

### Render resolution comparison tests:

Hardware: MacOS Sonoma, Asus ATI RX570 4GB, AMD Ryzen 5 5600x, 48GB RAM

Blender render:

- Above 900 × 500: "Out of GPU memory" after 8min.

SRT 6 splits (3 × 2): 

- At 1280 × 720: render finished after 23min.	
- At 1920 × 1080: GPU error in last split, after 27min. Rendered again (only last split), and finished after 9min. Total time: 38min.

SRT 24 splits (6 × 4): 

- At 1920 × 1080: render finished after 1h 12min.
- At 2560 × 1440: GPU error at split 15, after 59min. Rendered again (only last 10 splits), and finished after 42min. Total time: 1h 41min.

SRT 96 splits (12 × 8):

- At 2560 × 1440: render finished after 3h 23min.
- At 3840 × 2160: GPU error at split 48, after 2h 8min. Rendered again (from split 48), and finished after 2h 13min. Total time: 4h 21min.

SRT 192 splits (16 x 12):

- At 3840 × 2160: render finished after 5h 57min.

Blender rendered up to 880 × 460 (aprox.), while SRT rendered 3840 × 2160 on same computer, GPU, project and settings. Not tested 384 splits.

Obviously, the more the splits, the more the amount of time. With 6 splits almost finishes 1080 resolution, in 38 minutes (which means that at 12 splits is more than enough to finish it). But if you render with 24 splits the same resolution, it took almost double the time than with 6 splits. So it is important to adjust number of splits for each project, in order to minimize render times. 


How to install this addon:

- Download the zip file.
- Open Blender and go to Edit - Settings
- Go to Add-ons Tab.
- Press Install button, and search for the downloaded zip file.
- Select it and press Install
- Just click the box to enable it and you are ready to go.


How to use this addon:

First steps
	
- Due to the nature of SRT, the project file from within its executed will be slightly modified and saved. SRT automatically detects any failure
  on its processes, and shows a button to fix them (if is the case). Anyway, it is A GOOD PRACTICE to duplicate your original project (to have a
  backup of it) before use SRT.


Split Render
	
- Set your scene settings as usual (file format, resolution, start and end frames, passes, etc)
- Select number of splits in addon preferences (I've managed to render a 16k image with 6 splits in an AMD RADEON 570 4GB, but it depends on the
  complexity of the project too) (NOTE: the more splits, the more time will spent to finish the image)
- And click Render image or Render animation button
- You can separate passes into different files (Only OpenEXR Multilayer format) by enabling Separate layers
- You can enable Use Blender's output, to render files where you want (if output is a folder, the name of files will be "srt_xxxx.xxx")
- You can enable Don't delete cropped files to preserve , split files


Animated border	

- Set your scene settings as usual (file format, resolution, start and end frames, passes, etc).
- Jump to the frame you want to add key, expand Animated border settings tab, set the crop region to your liking, and press Add keyframe button.
- You need at least 2 keyframes (obviously) to Bake the animation (in order to setup all frames between keyframes).
- Then you can press Render animated border button (it will render from start frame to end frame)
- You can enable Create final composition file, and you will end up with a new Blender file called as the original one, plus "_COMPOSITION"
  (ex: AmazingCube.blend > AmazingCube_COMPOSITOR.blend). There will be two image nodes, the second one is connected to some nodes in order to
  blur the edges (The first one will be your original render)
- You can enable Use ellipse mask, so the blur will be based on an ellipse shape instead of square shape
- You can use the slider to control the blur size on edges (in pixels)
	(NOTE: Because it is in pixels, you will need to adjust this value depending on final resolution. 15-30 pixels is ok for 1920x1080 resolution)

