---
# Front matter. This is where you specify a lot of page variables.
layout: default
title:  "Digit Safe Planning"
date:   2024-04-14 21:00:00 -0500
description: >- # Supports markdown
  Provably-Safe, Real-time Planning & Control For Bipedal Robots Using Reachability-Based Trajectory Design
show-description: true

# Add page-specifi mathjax functionality. Manage global setting in _config.yml
mathjax: false
# Automatically add permalinks to all headings
# https://github.com/allejo/jekyll-anchor-headings
autoanchor: false

# Preview image for social media cards
image:
  path: ./assets/SafeDigit-Summary-3.0.png
  height: 285
  width: 800
  alt: SafeDigit Main Figure

# Only the first author is supported by twitter metadata
authors:
  - name: Bohao Zhang
    email: jimzhang@umich.edu
  - name: Ram Vasudevan
    email: ramv@umich.edu

# If you just want a general footnote, you can do that too.
# See the sel_map and armour-dev examples.
author-footnotes:
  All authors affiliated with the Robotics Institute of the University of Michigan, Ann Arbor.

links:
  - icon: arxiv
    icon-library: simpleicons
    text: ArXiv
    url: https://arxiv.org/abs/2402.08857
  - icon: github
    icon-library: simpleicons
    text: Code (Trajectory Optimization)
    url: https://github.com/roahmlab/IDTO
  - icon: github
    icon-library: simpleicons
    text: Code (Planning)
    url: https://github.com/roahmlab/SafeDigit

# End Front Matter
---

<!-- BEGIN DOCUMENT HERE -->

{% include sections/authors %}
{% include sections/links %}

---

# [Overview](#overview)

![reachable set construction](./assets/SafeDigit-Summary-3.0.png)
{: class="fullwidth"}

<!-- BEGIN OVERVIEW VIDEOS -->
<!-- <div class="fullwidth video-container" style="flex-wrap:nowrap; padding: 0 0.2em">
  <div class="video-item" style="min-width:0;">
    <video
      class="autoplay-on-load"
      preload="none"
      controls
      disablepictureinpicture
      playsinline
      muted
      loop
      style="display:block; width:100%; height:auto;"
      poster="assets/thumb/SafeDigit_single_arm_demo.jpg">
      <source src="assets/SafeDigit_single_arm_demo.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
    <p>SafeDigit performing single arm planning </p>
  </div>
  <div class="video-item" style="min-width:0;">
    <video
      class="autoplay-on-load"
      preload="none"
      controls
      disablepictureinpicture
      playsinline
      muted
      loop
      style="display:block; width:100%; height:auto;"
      poster="assets/thumb/SafeDigit_two_arm_demo.jpg">
      <source src="assets/SafeDigit_two_arm_demo.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
    <p>SafeDigit performing two arm planning </p>
  </div>
</div>  -->
<!-- END OVERVIEW VIDEOS -->

<!-- BEGIN ABSTRACT -->
<div markdown="1" class="content-block justify grey">

# [Abstract](#abstract)
To operate robustly in uncertain environments with a finite sensor horizon, bipedal robots must be able to perform receding horizon trajectory planning in concert with rapid control synthesis. 
Unfortunately, creating safe and dynamically-feasible trajectories for high-dimensional bipedal robotic systems in real-time is challenging. 
As a result, most existing approaches make an undesirable trade-off between model fidelity and planning speed thereby sacrificing safety.
To avoid this undesirable trade-off, this paper describes a method for offline generation of a continuum of multi-step reference trajectories that can then be optimizing over at run-time to identify trajectories that can be followed by a bipedal walking robot in a provably safe fashion (i.e., without running into obstacles or falling). 
To accomplish this objective, this paper presents a robust controller that provides an ultimate bound on the maximum tracking error while following any reference trajectory. 
At runtime and during each planning , the proposed method constructs a reachable set of the entire robot and intersects it with obstacles to generate sub-differentiable and provably conservative collision avoidance constraints. 
This enables trajectory optimization for an arbitrary cost function subject to these constraints. 
This proposed method consistently outperforms state-of-the-art methods during simulation experiments on a 20-degree-of-freedom bipedal robot Digit.

</div> <!-- END ABSTRACT -->

<!-- BEGIN METHOD -->
<div markdown="1" class="justify">

# [Method](#method)

<!-- # Contributions -->
As illustrated in the overview figure, this paper proposes a receding horizon control synthesis method that constructs a trajectory-parameterized reachable set of a bipedal robot over one walking step. 
During online operation, this paper describes how to construct a numerical optimization problem to identify a trajectory that ensures that the biped does not fall over or collide with any obstacles. 
Due to the application of the reachable set within the optimization problem, any feasible solution to the optimization problem can be realized on the biped in a provably safe fashion.
In particular, this paper describes the construction and application of a robust control input to track numerically computed trajectories with bounded tracking error under model uncertainty. 
To ensure that this optimization problem can be solved rapidly, this paper describes how to compute numerical derivatives for all relevant components of the optimization problem. 

This paper's contributions are four-fold:
1. A trajectory optimization algorithm based on inverse dynamics that generates periodic gaits and both converges faster and compute faster. 
2. An approach to compute trajectory-parameterized outer approximations of reachable sets that describe a bipedal robot's enclosure over time and over multiple steps while ensuring that this representation is differentiable.
3. A robust controller for a bipedal robot whose performance is proven to be ultimately bounded given model uncertainty.
4. An optimization-based planning algorithm that is illustrated in simulation to enable real-time safe navigation of high dimensional biped through complex environments. 

![reachable set construction](./assets/reachset-demo.jpg)
{: class="fullwidth"}

This figure demonstrates how we construct the reachable set on the lower limbs of Digit step by step. The first figure shows the reachable set of two joints in one time interval, constructed by Theorem \ref{thm-reachset-joint}. The second figure shows the reachable set of the link by connecting two joint reachable sets, constructed by Theorem \ref{thm-reachset-link}. The third and the fourth figure extend this to all time intervals and then all links, constructed by Theorem \ref{thm-reachset-wholebody}.

![step over demonstration](./assets/step_over.png)
{: class="fullwidth"}

This figure shows how Digit steps over one low obstacle plotted in red in one walking step, together with the robot reachable set corresponding with the optimal trajectory parameter that is found by the online optimization.
The robot reachable set is plotted in blue, and is not in collision with the low obstacle.

</div><!-- END METHOD -->

<!-- START RESULTS -->
<div markdown="1" class="content-block grey justify">

# [Simulation Results](#simulation-results)

To test the performance of our method, we generate 100 scenarios with 10 high obstacles and 20 low obstacles.
The overall success rate is 77% (More results included in Table IV in the paper).
In each of the demonstration videos below, SafeDigit is able to acheive the desired goal configuration without colliding with any obstacles.

<!-- START RANDOM VIDEOS -->
<div class="video-container">
  <div class="video-item">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      poster="assets/result_15.png">
      <source src="https://github.com/Cfather/DigitSafePlanning/assets/41474684/94da9bc5-3682-4018-a1bc-5d3b125f3a4d" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      poster="assets/result_21.png">
      <source src="https://github.com/Cfather/DigitSafePlanning/assets/41474684/e7adff7b-5764-4fcf-932d-a135ff689ee0" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      poster="assets/result_64.png">
      <source src="https://github.com/Cfather/DigitSafePlanning/assets/41474684/76bfa8e5-ed90-416b-b1b1-3321ce12194b" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      poster="assets/result_70.png">
      <source src="https://github.com/Cfather/DigitSafePlanning/assets/41474684/0003c6ed-3eac-458f-b6f8-5ea734122ac8" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div><!-- END RANDOM VIDEOS -->

</div><!-- END RESULTS -->

<div markdown="1" class="justify">
  
# [Related Projects](#related-projects)
  
* [Autonomous Robust Manipulation via Optimization with Uncertainty-aware Reachability](https://roahmlab.github.io/armour/)

* [Safe Planning for Articulated Robots Using Reachability-based Obstacle Avoidance With Spheres](https://roahmlab.github.io/sparrows/)

<div markdown="1" class="content-block grey justify">
  
# [Citation](#citation)

This project was developed in [Robotics and Optimization for Analysis of Human Motion (ROAHM) Lab](http://www.roahmlab.com/) at University of Michigan - Ann Arbor.

```bibtex
@article{zhang2024SafeDigit,
  title={Provably-Safe, Real-time Planning & Control For Bipedal Robots Using Reachability-Based Trajectory Design},
  author={Bohao Zhang and Ram Vasudevan},
  journal={ArXiv},
  year={2024},
  volume={abs/2402.08857},
  url={https://arxiv.org/abs/2402.08857}}
```
</div>


<!-- below are some special scripts -->
<script>
window.addEventListener("load", function() {
  // Get all video elements and auto pause/play them depending on how in frame or not they are
  let videos = document.querySelectorAll('.autoplay-in-frame');

  // Create an IntersectionObserver instance for each video
  videos.forEach(video => {
    const observer = new IntersectionObserver(entries => {
      const isVisible = entries[0].isIntersecting;
      if (isVisible && video.paused) {
        video.play();
      } else if (!isVisible && !video.paused) {
        video.pause();
      }
    }, { threshold: 0.25 });

    observer.observe(video);
  });

  // document.addEventListener("DOMContentLoaded", function() {
  videos = document.querySelectorAll('.autoplay-on-load');

  videos.forEach(video => {
    video.play();
  });
});
</script>

