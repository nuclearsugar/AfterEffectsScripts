# Rendering Multiple Instances of Aerender
After Effects is a lightly threaded app when rendering, even with Multi-Frame Rendering enabled. So if you have a good amount of RAM, then you can spin up multiple After Effects render engines at the same time. This is possible through the use of the [Aerender](https://helpx.adobe.com/after-effects/using/automated-rendering-network-rendering.html) executable, which has no GUI and can only be interacted with via a command line prompt.

## When is this technique necessary?
- If you have tons of comps that you need to render out and you have a powerful computer that can handle rendering multiple instances of Aerender at the same time. You'll need a powerful CPU and at least 32GB of RAM. You’ll want all of your footage to be on an SSD. Otherwise you’ll thrash your HHD and it’ll become a read/write bottleneck for the render speeds. The easiest way to hand off data to a different drive is to use the _______ feature.
- If you’re rendering somewhere over 600+ comps, then you’ll run into a knwon bug with After Effects 2026 and prior versions. If you try to add too many comps into the render queue, then the GUI with overflow and glitch out. This is because the current UI framework only supports a panel being 30,000 pixels wide or long. I've reported this [bug](https://community.adobe.com/bug-reports-528/too-many-comps-will-glitch-gui-1554202) to Adobe.
- Check out my [Isosceles_RenderQueueSplitIntoSeperateProjects.jsx](https://raw.githubusercontent.com/nuclearsugar/AfterEffectsScripts/refs/heads/main/Isosceles_RenderQueueSplitIntoSeperateProjects.jsx) script which takes all selected comps in the Project panel, splits them into batches of 10, adds each batch to the Render Queue, and saves a separate After Effects project file for each batch. This is useful because the After Effects render engine can slow down after rendering continuously for a long time. My theory is that the RAM is getting fragmented due to the fact that I'm frequently maxing out the available RAM. This happens especially since I'm often running multiple instances of the command line rendering via aerender. The hardware load balancer in After Effects is quite good, but sometimes a bit shaky when running x4 instances of aerender concurrently. So by reloading the whole After Effects render engine, it stays refreshed and renders without slow downs. 

# Workflow (on Windows 11)

## 1) Tweak the After Effects Settings
- Open up After Effects >>> Settings >>> Memory and Cache >>> Reduce to _________. These settings are also used by the Aerender executable.
- Reasoning: RAM is useful in After Effects for previewing purposes. But when it comes to rendering, After Effects doesn’t actually require a huge amount of RAM and it can render without any issue when using the minimum settings. Even when rendering at 3840x2160 at 60fps.

## 2) Prep the Render Queue
- You will need to prepare one After Effects for each render instance you plan on running.
- Open up your After Effects projects and add comps into the render queue. Set all of your render settings and file destinations as per usual.
- For example: Suppose you have a After Effects project with 400 comps. Then AE Project 1 will have comps 1-100 in the render queue, AE Project 2 will have comps 101-200 in the render queue, AE Project 3 will have comps 201-300 in the render queue, and AE Project 4 will have comps 301-400 in the render queue,
- When your done with this task, then you should close After Effects.

## 3) Prep your BAT Files
- Open up the [Notepad++](https://notepad-plus-plus.org/) app and create some new documents. The amount depends on how many Aerender instances you’ll be running. Save these file to your desktop and name it <Batch-Render-Instance1.bat>, <Batch-Render-Instance1.bat>, <Batch-Render-Instance1.bat>, and <Batch-Render-Instance1.bat>. Note that the <.bat> file extension is important since it allows you to quickly and easily get the command prompt booted up with the necessary settings pre filled out.
- Paste in the code listed below into each of the <Batch-Render-Instance#.bat> files. You’ll need to edit the text so that it uses the version of After Effects that you have installed and also change it so that it points directly to you After Effects project.

<Batch-Render-Instance1.bat>
```
title Batch-Render-Instance1
cd C:\Program Files\Adobe\Adobe After Effects 2026\Support Files
aerender -project C:\Users\SuperDuper\Desktop\Noise_v026.aep
```

<Batch-Render-Instance2.bat>
```
title Batch-Render-Instance2
cd C:\Program Files\Adobe\Adobe After Effects 2026\Support Files
aerender -project C:\Users\SuperDuper\Desktop\Noise_v027.aep
```

<Batch-Render-Instance3.bat>
```
title Batch-Render-Instance3
cd C:\Program Files\Adobe\Adobe After Effects 2026\Support Files
aerender -project C:\Users\SuperDuper\Desktop\Noise_v028.aep
```

<Batch-Render-Instance4.bat>
```
title Batch-Render-Instance4
cd C:\Program Files\Adobe\Adobe After Effects 2026\Support Files
aerender -project C:\Users\SuperDuper\Desktop\Noise_v029.aep
```

## 4) Ready to Render
- When you’re ready to begin rendering, double check on each of the <.BAT> files.
- Open the the Task Manager and keep a close eye on your _______. If you see the page file __________ rise dramtically, then you'll likely see one of the Aerender engines crash, stop rendering, and you'll see an alert in the relevant command prompt.
- In my experience, I can run x4 instances of Aerender and with my 32GB of RAM. Each instance uses about 5GB. Depending on the complexity of the scene that you’re rendering, you might need to lower down to x3 or x2 instances of Aerender if your CPU is pegged at 100% continuously. You may actually get faster renders by not saturating every thread. Assuming your cooling and airflow are adequate, running at full utilization isn't harmful to the CPU. However slightly underfeeding the threads can sometimes improve overall efficiency and reduce bottlenecks, resulting in shorter render times.
- If you’re using an alternate encoding engine, such as [AfterCodecs](https://www.autokroma.com/AfterCodecs), then it’ll use an additional _____ GB of RAM for the AfterCodecs engine. And that’s on top of the 5GB that Aerender uses. I can only run x3 instances when rendering via AfterCodecs.

## 5) Need to Stop Rendering Early?
- If you need to stop the renders, there isn't a way to pause the renders. First close the command prompt windows. Then you need to open up the Task Manager and force quit each the processes named _____________.
- When you're ready to resume the renders, you'll need to open up the After Effects projects and manually remove the items from the render queue that have been completed.
- For very heavy renders, I'd reccomend rendering using a PNG sequence or JPG sequence since you can resume rendering from where the last frame left off. Although I'd suggest always deleting the last frame since the file can be incomplete or corrupted if the render engine happened to be force quitted while in the process of writing the file.
