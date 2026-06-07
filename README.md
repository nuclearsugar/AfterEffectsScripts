# After Effects Scripts by ISOSCELES
Here's my collection of JSX scripts for After Effects. Since I often work with hundreds or thousands of comps within [my projects](https://www.jasonfletcher.info/vjloops/), these scripts were created to automate repetitive production tasks. The aim is to reduce the amount of manual effort required so that I can instead focus my energies on creative experiments. These scripts save me a ton of time.

## Installation
After downloading a JSX script, move it to the following location:
- Windows: `Program Files\Adobe\Adobe After Effects <version>\Support Files\Scripts`
- MasOS: `Applications/Adobe After Effects <version>/Scripts/ScriptUI Panels`

## Usage
To run a JSX script within After Effects...

`Open After Effects >>> File menu >>> Scripts >>> choose a script to execute`

## Summary of Scripts
- Isosceles_ChangeAlphaToIgnore.jsx - This After Effects script will set “Alpha: Ignore” (within the Interpret Footage window) for all the selected footage items in the Project window.
- Isosceles_ChangeAlphaToPremultiplied.jsx - This After Effects script will set “Alpha: Premultiplied” (within the Interpret Footage window) for all the selected footage items in the Project window.
- Isosceles_ChangeAlphaToStraight.jsx - This After Effects script will set “Alpha: Straight” (within the Interpret Footage window) for all the selected footage items in the Project window.
- Isosceles_ChangeFootageFrameRate.jsx - This After Effects script will prompt the user for the desired frame rate and then apply it to all selected footage items in the Project window.
- Isosceles_ExtendCompsByNFrames.jsx - This After Effects script will look at the selected comps in the Project window and extend each of their respective timelines by n frames.
- Isosceles_MakeSeamlessLoop.jsx - This script will create a seamless loop (with user-defined crossfade length) for the selected comps in the project window. You must precompose the main comp before executing this script.
- Isosceles_MoveTimeIndicatorFrames.jsx - This script will open the selected comps in the Project window and move the Current Time Indicator to the target frame.
- Isosceles_MoveTimeIndicatorSeconds.jsx - This script will open the selected comps in the Project window and move the Current Time Indicator to the target time.
- Isosceles_PasteClipboardIntoComps.jsx - This After Effects script will paste the clipboard contents into each comp that is selected in the Project window. This has only been tested with pasting a layer (footage, adjustment layer, or solid layer). The was the original use-case. I would recommend executing this script only with 100 comps selected at a time. Any more and After Effects will freeze. Not sure why.
- Isosceles_PasteClipboardIntoFootage.jsx - This After Effects script will select the .MP4 layer and paste the clipboard contents. This will be applied to the comps that are selected in the Project window. This has only been tested with pasting an effect into an MP4 footage item within a comp. Only the first footage layer will be affected.
- Isosceles_PrecompAllSelectedComps.jsx - This script will precomp all layers into a single precomp. This will be applied to the selected comps in the Project window. This differs from the built-in way to do this in After Effects, since this technique is entirely automated.
- Isosceles_RandomizeRotation.jsx - This After Effects script will randomize all rotation keyframes of the selected layer within a comp.
- Isosceles_RandomizeSeeds.jsx - This After Effects script will look at the selected comps in the Project window and then randomize the seed value for all wiggle expressions (in all layers).
- Isosceles_RemoveExpressions.jsx - This After Effects script will remove all expressions from the selected comps in the Project window.
- Isosceles_RenameAndSerialize.jsx - Batch rename the selected comps in the Project window with user-defined prefix, suffix, numbering, padding, and separator.
- Isosceles_RenderQueueSplitIntoSeperateProjects.jsx - This is useful because the After Effects render engine can slow down after rendering continously for a long time. My theory is that the RAM is getting fragmented due to the fact that I'm frequently maxing out the available RAM. This happens especially since I'm often running multiple instances of the command line rendering via aerender. The hardware load balancer in After Effects is quite good, but sometimes a bit shaky when running x4 instances of aerender concurrrently. So by reloading the whole After Effects render engine, it stays refreshed and renders without slow downs. I often render multiple instances of the command line rendering via aerender because if I have thousands of comps that need render out, then I will experience a known bug in After Effects where the current UI framework only supports a panel being 30,000 pixels wide or long. Any panel which overflows that will glitch. I've reported this [bug](https://community.adobe.com/bug-reports-528/too-many-comps-will-glitch-gui-1554202) to Adobe.
- Isosceles_StretchFootageInComps.jsx - This After Effects script will apply a user-defined Stretch percentage to all footage layers within selected comps in the Project window.
- Isosceles_TrimCompToWorkArea.jsx - This script will trim the comp to its work area. This operation will be executed for the selected comps in the Project window.
- Isosceles_WorkAreaEndShortenByNFrames.jsx - This After Effects script shortens the work area end point by a user-defined number of frames for selected comps in the Project window.
- Isosceles_WorkAreaEndToFootageEnd.jsx - This After Effects script will move the "Work Area End" to the end of the contained footage of the comp. This will be applied to the comps that are selected in the Project window.
- Isosceles_WorkAreaStartToFootageStart.jsx - This After Effects script will move the "Work Area Start" to the earliest contained footage start in the comp. This will be applied to the comps that are selected in the Project window.
- rd_RenderLayers_IsoscelesFix.jsx - Moves the "Work Area Start" to a user-defined frame for all selected comps in the Project window.
rd_RenderLayers_IsoscelesFix.jsx - This is a well known script within the Motion Design community and is very useful script in certain situations. But it had been broken for several years due to changes in After Effects and so it wasn't usable. I did some tests and was able to determine what was causing the issue. So I crafted up a text prompt and ChatGPT was able to fix the script after a few clarifications. Although there was no license attached to this script, the original authors website is offline <redefinery.com>, and yet the broken version of the script is freely distributed on <aescripts.com>. That left me in a bit of an awkward situation regarding distribution of the modified version of this script. But a comment included within the script from the original author gave a feeling of openness: "I'm just trying to share knowledge with and help out my fellow AE script heads". And so with that in mind, I've decided to distribute this modified version of the script using the GPL-3.0 license.
