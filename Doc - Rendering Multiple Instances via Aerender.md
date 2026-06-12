# Rendering Multiple Instances via Aerender
Adobe After Effects (AE) is typically a lightly threaded app while rendering, even with [Multi-Frame Rendering](https://helpx.adobe.com/after-effects/using/multi-frame-rendering.html) enabled. This means that your CPU cores likely aren't being fully utilized and you have the option of running multiple AE render engines at the same time. This is possible through the use of the [Aerender](https://helpx.adobe.com/after-effects/using/automated-rendering-network-rendering.html) executable, which is installed by default, but has no GUI and can only be interacted with via a command line prompt. This works fine even when your comps are using AE plugins which you've purchased. This is a guide of how to get multiple render instances rolling. No special tools need to be installed.

## When is this technique necessary?
- **Possibility 1:** This technique is ideal if you have tons of comps that you need to render out and you have a powerful computer that can handle rendering multiple instances. You'll need a powerful CPU and at least 32 GB of RAM. You’ll want all of your footage to be on an SSD. Otherwise you’ll thrash your HDD and it’ll become a read/write bottleneck for the render speeds. The easiest way to hand off an AE project to a different drive is to use the "Collect Files" feature (File menu >>> Dependencies >>> Collect Files)
- **Possibility 2:** If you’re rendering somewhere over 600+ comps, then you’ll run into a known bug with After Effects 2026 (and prior versions) where if you try to add too many comps into the render queue then the GUI will overflow and glitch out. This is because the current UI framework only supports a panel being 30,000 pixels wide or long. I've reported this [bug](https://community.adobe.com/bug-reports-528/too-many-comps-will-glitch-gui-1554202) to Adobe and the dev team is working on a fix. So instead we can break up the render queue into different AE project files and then render it out using multiple instances or [back-to-back](https://github.com/nuclearsugar/AfterEffectsScripts/blob/main/Doc%20-%20Rendering%20Multiple%20Instances%20via%20Aerender.md#tip-render-multiple-ae-projects-back-to-back).
- **Possibility 3:** If you're rendering for more than 24 hours, the AE render engine will gradually slow down over long uninterrupted renders. After 48 hours of continuous rendering, I frequently see the per-frame render times slow down by a factor of two and yet I know that the complexity of the comps is consistent. So by reloading the whole AE render engine every so often, it will maintain consistent performance and avoid slow downs.
- **Possibility 4:** This technique also allows you to render in the background via Aerender and then continue working on a different project in After Effects. Just need to be sure you have enough RAM to allow for this.

# Workflow
I've used this technique extensively on Windows 11.

## Step 1) Tweak the AE Preferences
- AE >>> Edit menu >>> Preferences >>> Memory & CPU >>> Increase the "RAM reserved for other applications" to the highest possible value. This will leave 1 GB of RAM available as a frame buffer, but the AE render engine will use more RAM than this as required to run. These settings are also used by the Aerender executable. I like to take a screenshot of these settings so that I can set it back to normal after the renders have completed, otherwise your RAM previews will be very short. The reasoning behind this settings adjustment is that RAM is useful in AE for previewing purposes. But when it comes to rendering, AE doesn’t actually require a huge amount of RAM and it can render without any issue when using the minimum settings, even when rendering at 3840x2160 at 60fps.
- AE >>> Edit menu >>> Preferences >>> Memory & CPU >>> Enable "Multi-Frame Rendering". Set the "% CPU reserved for other applications" to 10%. The hardware load balance within the AE render engine will automatically share the CPU threads between the multiple instances, but you have to give it a ceiling to work with.

## Step 2) Prep the Render Queue for each AE Project
- You need to prepare one AE project for each render instance that you plan on running. For this tutorial I will be running x4 instances.
- Open up each of your AE projects and add comps into the render queue. It could be the exact same AE project and just duplicated four times. Set all of your render settings and file destinations as per usual in the built-in AE render queue. When you're done with this task then you should close AE to free up the RAM.
- For example: Suppose you have an AE project with 400 comps. Then <AE-Project-1.aep> will have comps 1-100 in the render queue, <AE-Project-2.aep> will have comps 101-200 in the render queue, <AE-Project-3.aep> will have comps 201-300 in the render queue, and <AE-Project-4.aep> will have comps 301-400 in the render queue,
- To shortcut this process, check out the [ISO_RenderQueueSplitIntoSeperateProjects.jsx](https://raw.githubusercontent.com/nuclearsugar/AfterEffectsScripts/refs/heads/main/ISO_RenderQueueSplitIntoSeperateProjects.jsx) script which takes the selected comps in the Project window, splits them into fixed-size batches (N per batch), adds each batch to the Render Queue, and saves a separate After Effects project file for each batch.

## Step 3) Prep your BAT Files
- Open up the [Notepad++](https://notepad-plus-plus.org/) app (or any text editor) and create 4 new documents. Save these files to your desktop and name them <Batch-Render-Instance1.bat>, <Batch-Render-Instance2.bat>, <Batch-Render-Instance3.bat>, and <Batch-Render-Instance4.bat>.
- Note that the <.bat> file extension is important here since it allows you to quickly get the command prompt window started up with the necessary settings.
- Paste in the code listed below into each of the <Batch-Render-Instance#.bat> files. You’ll need to edit the text so that it uses the version of AE that you have installed and also change it so that it points directly to each one of your AE projects.

<Batch-Render-Instance1.bat>
```
title Batch-Render-Instance1
cd /d "C:\Program Files\Adobe\Adobe After Effects 2026\Support Files"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v026.aep"
echo.
echo All renders completed.
pause
```

<Batch-Render-Instance2.bat>
```
title Batch-Render-Instance2
cd /d "C:\Program Files\Adobe\Adobe After Effects 2026\Support Files"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v027.aep"
echo.
echo All renders completed.
pause
```

<Batch-Render-Instance3.bat>
```
title Batch-Render-Instance3
cd /d "C:\Program Files\Adobe\Adobe After Effects 2026\Support Files"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v028.aep"
echo.
echo All renders completed.
pause
```

<Batch-Render-Instance4.bat>
```
title Batch-Render-Instance4
cd /d "C:\Program Files\Adobe\Adobe After Effects 2026\Support Files"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v029.aep"
echo.
echo All renders completed.
pause
```

## Step 4) Ready to Render
- When you’re ready to begin rendering, double-click on each of the <.bat> files. A command prompt window will open for each render instance and it will begin rendering automatically.
- If you instantly see an error in the command prompt window, then you probably didn't fill out the details correctly during Step 3.
- If your RAM becomes too bloated then one of the AE render engines will crash and you'll see an alert in the relevant command prompt. If this happens then you should reduce the amount of instances you're running by one and try again.
- In my experience, I can run x4 instances with my 32GB of RAM and each instance uses about 5 GB. Depending on the complexity of the AE project that you’re rendering, you might need to only run x3 or x2 instances if all CPU cores are continuously maxed out at 100%. Ironically you will likely get faster rendering by not fully saturating every CPU thread. Assuming your cooling and airflow are adequate, running at full utilization isn't harmful to the CPU, however slightly underfeeding the CPU threads can improve overall efficiency and reduce bottlenecks.
- If you’re using an alternate encoding engine, such as [AfterCodecs](https://www.autokroma.com/AfterCodecs), then it’ll use an additional 1-2 GB of RAM for the AfterCodecs engine. And that’s on top of the 5 GB that the AE render engine already uses. I can only run x3 instances when rendering via AfterCodecs.
- If you're a performance junkie like me, then open the Task Manager >>> Performance tab >>> and watch the CPU and Memory tabs. The memory can safely hover around 98% utilization. And even then, memory can be cached to some extent without any slow downs (I've seen up to 8 GB cached). I theorize this is because most of this memory is just the temporary frame buffer, source footage decoding, or filesystem read-ahead. It's difficult to know and doesn't seem to really matter anyways. So don't pay too much attention to the "Cached" stat on the Memory tab. But having a large "Paged Pool" stat would be bad, but I think that AE will crash before that happens.

## Need to Stop Rendering Early?
- If you need to stop rendering prematurely, first close the command prompt windows. Then open up the Task Manager >>> Processes tab >>> and force quit each process (x4) named "Adobe After Effects 2026".
- Beware: the typical CTRL+C hotkey which stops a process running in a command prompt window, actually does not end the "Adobe After Effects 2026" process. The process must be stopped manually.
- If you need to resume the renders, you'll need to open up the AE projects and manually remove the items from the render queue that have been completed. It's not ideal, but that's just a caveat of this technique.
  - I keep forgetting to test it, but I've read that if you're rendering to a frame sequence then you can enable "Skip Existing Files" in the Render Settings window and then you don't need to edit the render queue before resuming a render. But beware of the next bullet.
- For very heavy renders or long duration comps, I'd recommend rendering using a PNG sequence or JPG sequence since you can resume rendering from where the last frame left off. Although when resuming a render, always delete the last frame since the file could be incomplete or corrupted if the AE render engine happened to be force quitted while in the process of writing the file.

## Tip: Render Multiple AE Projects Back-to-Back
- You can string together multiple AE projects so that they render back-to-back. This is effectively a queue of AE projects, with each AE project containing its own render queue. A queue of queues... blasphemy!
- This is useful if you have tons of different AE projects that need to be rendered out.
- It's also useful if you have thousands of comps within a single AE project. In this case, check out this script: [ISO_RenderQueueSplitIntoSeperateProjects.jsx](https://raw.githubusercontent.com/nuclearsugar/AfterEffectsScripts/refs/heads/main/ISO_RenderQueueSplitIntoSeperateProjects.jsx)
- Any computer that can render AE can handle this setup since it's just a single instance. For example:

<Batch-Render-Instance1.bat>
```
title Batch-Render-Instance1
cd /d "C:\Program Files\Adobe\Adobe After Effects 2026\Support Files"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v094.aep"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v095.aep"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v096.aep"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v097.aep"
aerender -project "C:\Users\Isosceles\Desktop\Noise_v098.aep"
echo.
echo All renders completed.
pause
```
