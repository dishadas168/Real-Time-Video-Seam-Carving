# Real-Time-Video-Seam-Carving
# Abstract
This project attempts combining multiple methods to speed up the Seam Carving algorithm so that it can be run to remove objects from videos in real time.  Methods include algorithmic and hardware optimizations, some sourced from extant papers and some were novel ideas.  We attempted three major categories of seam carving speedups: algorithmic video optimization based on the work by Zhu, forward energy calculation methods based on the work by Rubinstein, Shamir, and Avidan, hardware optimization using MatLab’s matrix math and parallel processing capabilities (inspired by the work by Duarte and Sendag) with some algorithmic changes to reduce the compute time.

In our final phase of testing we additionally assessed object detection and tracking methods. We leveraged SIFT object detection to find, and subsequently remove, an object. SIFT required approximately 5 seconds per frame, and could not operate at real-time.

In the final version of the application, simple circle detection and a variant of the spiral model were successfully employed for real-time tennis ball detection and deletion using seam carving. The whole application was then built using the MATLAB Rasperry Pi support, and deployed to a Raspberry Pi 3 B+ with a camera. Simple circle detection could find the tennis ball, and the spiral model could then remove it, but on the Pi it did not run within the minimum successful time. On a moderate device, the times were around .1 seconds per frame; on an 8th gen core i7 with an NVIDIA GeForce GTX 1080 and 32 GB of RAM, it ran at approximately .07 seconds per frame. The tennis ball detection was the only one that ran in real time with reasonable results.

# Introduction
Seam carving, as introduced in 2007 by Shai Avidan and Ariel Shamir, (available here), is a method of context-aware resizing.

# Applications
One of the applications by the original research team was to remove a selected section of an image, and this is the portion we plan to replicate. Applications include removing unwanted objects while live-streaming a video, or modifying incoming images on a heads-up display to not see indicated items.  If you built an AR game and it used this idea very successfully, you could have, for example, objects that appear to float because you can’t see the support, or you could remove obvious tracking/reference tags from an AR game playing area

# Requirement
The results will be considered "real time" when the latency is under a reasonable limit for a heads-up display. We'll take our cues from this paper, which suggests that generally a 50ms delay is acceptable, and up to a 100ms delay can be unnnoticed by many people. That is, a basic level of success will be considered achieved if the system produces less than a 100ms delay, although the goal will be less than 50ms. In general, the approach will be defined by the following three stages: optimizing the static algorithm, optimizing with hardware, and optimizing for video.

# Baseline Images
![img1](img1.png)
![img1](img1.png)
![img1](img1.png)

# Spiral Model
![img1](img1.png)
![img1](img1.png)
![img1](img1.png)

# Results for a video
![img1](img1.png)
![img1](img1.png)
![img1](img1.png)
![img1](img1.png)

Total running time for video seam carving

Average run time = 109.8954 seconds for 20 frames
        = 5.49 seconds for each frame
        
# Simple object detection
using a Hough transform circle detector to recognize the tennis ball in images and videos. The MatLab implementation of this, imfindcircles, was marginally faster than our implementation, and so we did use that method. We tuned the system, however, for sensitivity as well as to accept a range of circle sizes, and used our image tests to find values and ranges that would work well in the majority of our test cases without slowing the processing time outside of our target time. The values that worked against all four of our sample images, and worked adequately against video, were size ranges between 8 and 22 pixels, with sensitivity at .9 rather than MatLab's default .8. Processing a larger range than that significantly slowed down our system, and processing a smaller range caused the tennis ball to often re-appear when the camera was moved.

![img1](img1.png)
![img1](img1.png)

# Summary
In conclusion, we fundamentally met our stated minimum goal, to remove a tennis ball from a video at a real-time frame rate, specifically less than .1 second of processing time per frame/image. The area most easily optimized for speed improvements from this point is likely detection and tracking, although further optimization of the seam carving algorithm itself may be of value in reducing processing time. We did not meet our ideal goal, of processing at less than .05 (50ms) per frame. 

