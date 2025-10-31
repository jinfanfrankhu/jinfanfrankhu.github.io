---
layout: project
title: AI-Powered Crosswalk Safety Monitoring System
image: /images/projects/crosswalk/cover.JPG
description: Built a dual-model computer vision system using Raspberry Pi 5 and IMX500 to detect pedestrians and vehicles at a dangerous crosswalk. Solved real tracking problems like "ghost cars" and distant pedestrian detection.
permalink: /projects/crosswalk-detection/
display: true
tags:
  - "Computer Vision"
  - "Object Detection"
  - "Raspberry Pi"
  - "YOLO"
  - "Real-time Systems"
  - "Traffic Safety"
keywords: "computer vision, object detection, YOLO, pedestrian detection, crosswalk safety, Raspberry Pi, IMX500, traffic monitoring, IoU tracking"
date: 2025-10-30
---

This project started because I got tired of waiting for cars (and getting early-morning class tardies) every single day. I lived in America House during junior year, the furthest underclassmen dorm on Andover's campus, and crossing MA State Highway 125 was a daily nightmare. The crosswalk signals were terrible: they'd go off at the wrong times, block traffic when no one was crossing, and generally made everyone's lives worse.

So I did what any reasonable person would do: I emailed the town government. They said it was the state's responsibility. I emailed the state government. They said they needed a traffic study first. Cool, cool. Guess I'll just build my own traffic monitoring system then.

## Wait, you built a what now?

TLDR: An AI-powered system that uses computer vision to detect and track pedestrians and vehicles at crosswalks. It runs on a Raspberry Pi 5 with an IMX500 camera and logs crossing patterns to support advocacy for better traffic signals.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>The system is basically a smart camera that watches a crosswalk and logs every time a pedestrian crosses or a vehicle passes through. It uses two AI models working together: the IMX500's built-in nanodet_plus model for general detection (especially good at vehicles), and YOLOv8n for enhanced pedestrian detection.</p>

<p>The key innovation is the dual-model pipeline. The IMX500 alone would miss pedestrians who were more than 15 feet away because they'd be too small in the frame. So I extract just the crosswalk area, upscale it 2.5x, and run it through YOLO. Then I merge the detections using IoU (Intersection over Union) to avoid counting the same person twice.</p>

<p>Everything runs in real-time on the Raspberry Pi, with a web interface at localhost:8080 showing live detections. The system logs events to separate files for pedestrians, vehicles, and combined events, all in JSONL format with timestamps and confidence scores.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## How does the hardware setup work?

TLDR: Raspberry Pi 5 + IMX500 AI camera module (which has a built-in neural accelerator) + power bank for portability. Sometimes I'd bring a ladder for better camera angles.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>The IMX500 is pretty special. It's a camera module with an integrated neural processing unit, meaning it can run AI models directly on the sensor itself. This is way more efficient than sending every frame to the Raspberry Pi's CPU for processing. The IMX500 handles the heavy lifting of running nanodet_plus on every frame.</p>

<p>For field testing, I'd set everything up on the sidewalk with a portable power bank. The trickiest part was getting the camera angle right. You need to see the entire crosswalk clearly, which sometimes meant I'd bring a small ladder to get elevation. Nothing says "I'm definitely not doing anything suspicious" like a teenager with a ladder and computer equipment on the side of a highway... I was asked a couple times by passersby's what I was doing, and I was so afraid that they'd think I'm planting a bomb on campus and call the cops on me.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## You mentioned a dual-model pipeline. Why use two models?

TLDR: The IMX500's nanodet_plus is excellent at detecting vehicles but misses distant pedestrians. YOLOv8n is better at finding people but is slower. Using both together gives the best of both worlds.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>Here's the problem I discovered during testing: when a pedestrian is 15+ feet away from the camera, they're only a few dozen pixels tall in the frame. The IMX500 running nanodet_plus would just... miss them. It would pick them up once they got closer, but by then you've lost valuable data about when they entered the crosswalk.</p>

<p>My solution was ROI (Region of Interest) upscaling. I defined the crosswalk area as a polygon in a JSON config file. For each frame, I extract just that area, upscale it 2.5x using nearest-neighbor interpolation (fast and good enough for this use case), and run YOLOv8n on the upscaled region. Now those distant pedestrians are big enough to detect reliably.</p>

<p>But this creates a new problem: now both models might detect the same person. That's where IoU-based merging comes in. For each pair of detections, I calculate their Intersection over Union. If IoU > 0.5 and they're the same object type (both "person" or both "car"), they're probably the same object, so I keep only the detection with higher confidence.</p>

<p>The result: reliable vehicle detection from the fast IMX500 model, enhanced pedestrian detection from the YOLO upscaling pipeline, and no duplicate counting.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What's this "ghost car" problem you mentioned?

TLDR: When a parked car sits near the crosswalk, the detector would flicker on and off detecting it. This would log the same car dozens of times. I solved it with distance-based cooldown tracking.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>This was genuinely one of the most annoying bugs I encountered. Picture this: there's a car parked 20 feet from the crosswalk. The detector sees it in frame 1, doesn't see it in frame 2 (maybe lighting changed, maybe the confidence dipped below threshold), sees it again in frame 3, doesn't see it in frame 4... you get the idea.</p>

<p>My naive tracking system would log a "vehicle entered zone" event every single time the detection flickered back on. One parked car over 30 seconds could generate hundreds of bogus log entries. I called them "ghost cars."</p>

<p>To fix this, I used distance-based cooldown tracking. The system now maintains a history of the last 10 positions for each tracked object. When a new detection comes in, I check: is this detection more than 100 pixels away from the last logged position for this object? If yes, it's genuinely a new object (or the same object has moved significantly). If no, it's probably just the detector flickering, so I ignore it until the cooldown expires.</p>

<p>This single change dropped false positive logging by a lot. The key insight was that real crosswalk events involve motion. A car entering the crosswalk moves at least several feet between frames. Detector flicker happens in place.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What's the weighted detection point thing?

TLDR: Instead of using the center of the bounding box to determine which zone an object is in, I use a point 75% down the box. This massively improved vehicle detection because cars' roofs can be outside a zone while their bumpers are inside it.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>Bounding boxes are rectangles drawn around detected objects. The obvious way to check if an object is in a zone is to check if the center point of its bounding box is inside the zone polygon. This works great for pedestrians.</p>

<p>But for vehicles? Not so much. A car's bounding box includes the entire vehicle from the ground to the top of the roof. The mathematical center of that box is somewhere around the roof line. But the part of the car that actually matters for crosswalk safety is the front bumper, which is at the bottom of the bounding box.</p>

<p>So I'd get situations where a car's front bumper was clearly inside the crosswalk zone, but its "center point" (up at the roof) was still outside the zone boundary. The system wouldn't log it as being in the crosswalk. Terrible.</p>

<p>The fix: instead of using the center point (50% down the bounding box), I use a weighted point at 75% down. For vehicles, this puts the detection point much closer to where the bumper is, which is the part we actually care about. For pedestrians, it doesn't matter much since they're roughly vertical.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## How do you define the zones themselves?

TLDR: I built a web tool called find_borders.py that shows the live camera feed. You click on the corners of zones, and it saves the coordinates to a JSON file. Super intuitive.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>Defining zone boundaries by manually typing in coordinates would be absolutely miserable. You'd have to guess pixel coordinates, run the system, see if the zone looks right, adjust coordinates, repeat. No thanks.</p>

<p>Instead, find_borders.py launches a web interface showing the live camera feed. You click on the corners of your zone (crosswalk, sidewalk, vehicle lanes, etc.) in order, and it displays the polygon in real-time as you build it. Once you're happy with it, you save it to a JSON file with a descriptive name.</p>

<p>The JSON files live in the zones/ directory and look like:</p>

```json
{
  "crosswalk": [[245, 180], [670, 185], [665, 420], [240, 415]],
  "vehicle_lane_north": [[100, 50], [800, 80], [780, 200], [110, 190]],
  "sidewalk_east": [[750, 200], [900, 210], [895, 600], [745, 590]]
}
```

<p>The main detection script loads these zone definitions and checks which zones each object is in every frame. You can easily switch between different zone configurations by loading different JSON files, which is great for testing different camera angles or monitoring different crosswalks.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What's the actual development workflow like?

TLDR: Identify problems through field testing, design algorithmic solutions, have Claude Code implement them in Python, iterate with more testing. 

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p>I used Claude Code (Anthropic's AI coding assistant) extensively for this project. The workflow went something like:</p>

<ol>
<li><strong>Field test</strong> - Set up the system at the actual crosswalk and watch what happens</li>
<li><strong>Identify problems</strong> - "Why is it logging the same parked car 50 times?" or "Why is it missing pedestrians?"</li>
<li><strong>Design solution</strong> - Figure out the algorithmic approach (distance-based cooldown, ROI upscaling, etc.)</li>
<li><strong>Implement via Claude</strong> - Have Claude write the Python code to implement the solution</li>
<li><strong>Debug and refine</strong> - Test the changes, identify edge cases, iterate</li>
</ol>

<p>Claude wrote almost all of the code of the code, but all the problem-solving and creative solutions came from my side. The AI is excellent at implementing well-specified algorithms and handling the tedious parts of Python syntax, OpenCV calls, and proper error handling. But it can't tell you that you need distance-based cooldown or weighted detection points. That requires actually understanding the domain problem.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What other tools did you build to support this?

TLDR: analyze_crossings.py for statistical analysis, record_video.py for capturing test footage with zone overlays, and photo_grid.py for taking reference photos with coordinate grids.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p><strong>analyze_crossings.py</strong><br>
This script reads the JSONL log files and generates statistical reports: How many pedestrians crossed per hour? What's the peak traffic time? What's the ratio of vehicles to pedestrians? How often do vehicles enter the crosswalk while pedestrians are present? This is the tool that actually turns raw detection logs into actionable data for advocacy.</p>

<p><strong>record_video.py</strong><br>
Sometimes you need to see what the system saw. This records video with the zone boundaries and detections drawn on each frame, which is incredibly useful for debugging. You can watch the recording and see exactly when and why the system made certain decisions.</p>

<p><strong>photo_grid.py</strong><br>
Takes a snapshot from the camera with a coordinate grid overlay. Super useful for planning zone layouts before you go into the field. You can look at a gridded photo and sketch out roughly where your zone boundaries should be, then use find_borders.py to click the exact points.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What's the current status and what's next?

TLDR: System is functional and field-tested. Successfully detects pedestrians and vehicles, logs crossing events, and has analyzed real crosswalk data. Next steps include directional detection, crossing duration analysis, and eventually presenting the data to authorities.

<details>
<img src="/images/cat-closed.png" style="display: block; margin: 0 auto;"><summary><strong>Click to expand a longer explanation.</strong></summary>

<p><strong>Current Status</strong><br>
The system works...? I've deployed it at the crosswalk for a bit, collected data, and confirmed that the detection rates are solid. The dual-model pipeline catches both distant pedestrians and vehicles reliably. The ghost car problem is solved. The weighted detection points correctly identify when vehicles enter the crosswalk zone.</p>

<p><strong>Future Improvements (from my todo.txt)</strong><br>
<ul>
<li><strong>Directional detection</strong> - Add sidewalk zones on all four sides so I can tell which direction pedestrians are traveling</li>
<li><strong>Complete crossing sequences</strong> - Track the full path: sidewalk east → crosswalk → sidewalk west, with timestamps for entry and exit</li>
<li><strong>Crossing duration</strong> - Calculate how long each crossing takes from entry to exit</li>
<li><strong>Better tracking consistency</strong> - Maintain tracking IDs more reliably across zone transitions</li>
<li><strong>Analytics dashboard</strong> - Build a web dashboard for visualizing patterns, peak hours, and safety metrics</li>
</ul>
</p>

<p><strong>The Big Goal</strong><br>
Eventually, I want to take all this data back to the town and state government with actual evidence: "Here's proof that X pedestrians cross during this time window, Y vehicles enter the crosswalk while pedestrians are present, and the current signal timing causes Z conflicts per hour." Real data is harder to ignore than complaints.</p>

<img src="/images/cat-open.png" style="display: block; margin: 0 auto;">

</details>

## What did you learn from this project?

This was my first significant engineering project, and honestly, it taught me more than any class could have. I learned that:

1. **Real-world problems are messy.** No textbook prepares you for "ghost cars" or the fact that a car's bounding box center is at its roof.

2. **Field testing is irreplaceable.** I could simulate detection pipelines all day, but I'd never discover the parked car problem without actually deploying the system and watching it run.

3. **Simple solutions often work best.** Distance-based cooldown solved the ghost car problem with like 20 lines of code. Weighted detection points was literally changing one number from 0.5 to 0.75.

4. **Iteration matters more than perfection.** The first version missed half the pedestrians. The second version logged the same car 100 times. The third version had zone boundary issues. Each iteration got better by addressing specific observed problems.

This project turned a daily frustration into an engineering challenge, and solving it felt genuinely rewarding. Plus, I now have a pretty cool traffic monitoring system that actually works.