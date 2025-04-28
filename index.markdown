---
# # Front matter. This is where you specify a lot of page variables.
# layout: default
# title:  "BLAZE"
# date:   2024-02-13 10:00:00 -0500
# description: >- # Supports markdown
#   Pseudospectral Heat fLow using the Affine geoMetric Equation
# show-description: true

# Front matter. This is where you specify a lot of page variables.
layout: default
title:  "Phasing Through the Flames"
date:   2024-10-17 10:00:00 -0500
description: >- # Supports markdown
  Rapid Motion Planning with the AGHF PDE for Arbitrary Objective Functions and Constraints
show-description: true

# Add page-specific mathjax functionality. Manage global setting in _config.yml
mathjax: false
# Automatically add permalinks to all headings
# https://github.com/allejo/jekyll-anchor-headings
autoanchor: false

# Preview image for social media cards
image:
  path: assets/BLAZE_cover_figure.pdf
  height: 600
  width: 800
  alt: BLAZE Main Figure

# Only the first author is supported by twitter metadata
authors:
  - name: Ram Vasudevan
    email: ramv@umich.edu

# If you just want a general footnote, you can do that too.
author-footnotes:
  All authors affiliated with the department of Robotics at the University of Michigan, Ann Arbor.

links:
  - icon: bi-file-earmark-text
    icon-library: bootstrap-icons
    text: Paper
    url: https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=10919247
  - icon: arxiv
    icon-library: simpleicons
    text: ArXiv
    url: https://arxiv.org/abs/2411.12962
  - icon: github
    icon-library: simpleicons
    text: Code
    url: https://github.com/roahmlab/BLAZE
  - icon: bi-file-earmark-text
    icon-library: bootstrap-icons
    text: Supplementary Appendices
    url: BLAZE_Appendices.pdf

# End Front Matter
---

<!-- BEGIN DOCUMENT HERE -->

{% include sections/authors %}
{% include sections/links %}

---

<!-- BEGIN OVERVIEW FIGURE -->
<!-- <div class="fullwidth video-container" style="display: flex; flex-wrap:nowrap; gap: 10px; padding: 0 0.2em; justify-content: center">
  <div class="video-item" style="flex: 1 1 50%; max-width: 50%;">
    <video
      class="autoplay-on-load"
      preload="none"
      controls
      disablepictureinpicture
      playsinline
      muted
      loop
      style="display:block; width:100%; height:auto;">
      <source src="assets/aghf_digit_stretch.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
    <p>Digit Doing Yoga Pose (Generated in ~3s) </p>
  </div>
  <div class="video-item" style="flex: 1 1 50%; max-width: 50%;">
    <video
      class="autoplay-on-load"
      preload="none"
      controls
      disablepictureinpicture
      playsinline
      muted
      loop
      style="display:block; width:100%; height:auto;">
      <source src="assets/aghf_digit_stair.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
    <p>Digit Stepping on Box (Generated in ~2s) </p>
  </div>
</div>  -->
<!-- END OVERVIEW VIDEOS -->


<!-- BEGIN ABSTRACT -->
<div markdown="1" class="content-block justify grey">

# [Abstract](#abstract)

The generation of optimal trajectories for high-dimensional robotic systems under constraints remains computationally challenging due to the need to simultaneously satisfy dynamic feasibility, input limits, and task-specific objectives while searching over high-dimensional spaces. 
Recent approaches using the Affine Geometric Heat Flow (AGHF) Partial Differential Equation (PDE) have demonstrated promising results, generating dynamically feasible trajectories for complex systems like the Digit V3 humanoid within seconds. 
These methods efficiently solve trajectory optimization problems over a two-dimensional domain by evolving an initial trajectory to minimize control effort. 
However, these AGHF approaches are limited to a single type of optimal control problem (i.e., minimizing the integral of squared control norms) and typically require initial guesses that satisfy constraints to ensure satisfactory convergence.
These limitations restrict the potential utility of the AGHF PDE especially when trying to synthesize trajectories for robotic systems.
This paper generalizes the AGHF formulation to accommodate arbitrary cost functions, significantly expanding the classes of trajectories that can be generated. 
This work also introduces a Phase 1 - Phase 2 Algorithm that enables the use of constraint-violating initial guesses while guaranteeing satisfactory convergence.
The effectiveness of the proposed method is demonstrated through comparative evaluations against state-of-the-art techniques across various dynamical systems and challenging trajectory generation problems.
</div> <!-- END ABSTRACT -->

<!-- BEGIN METHOD -->
<div markdown="1" class="justify">

# [Approach](#method)

![link_construction](assets/BLAZE_cover_figure.png)
{: class="fullwidth"}

<!-- # Contributions -->
It begins with an initial trajectory (shown in orange with the color gradient illustrating the evolution in time starting from darkest and going to lightest) that may violate constraints (e.g., the second and fourth pose of the arm are in collision with the boxes and outlined in red).
If the initial trajectory is infeasible, BLAZE enters Phase 1, where it evolves the trajectory into a trajectory that satisfies all constraints (e.g., in the blue trajectory, the Kinova arm has been moved out of collision with the boxes).
Once the trajectory satisfies all constraints, Phase 2 begins, optimizing the motion to minimize a user-specified cost function while maintaining feasibility (optimized trajectory shown green).
In this case, BLAZE optimizes the trajectory to reach a target configuration while avoiding the obstacles while considering the full dynamical model of the arm. 
Note that optimal control (including Phase 1 and Phase 2) for this 14 dimensional state space model is completed within $$3$$s while satisfying input, state, and collision avoidance constraints. 

This paper’s contributions are three-fold:
1. This paper illustrates how to formulate the AGHF Action Functional to solve optimal control problems with arbitrary cost functions
2. This paper illustrates how to implement a Phase 1-Phase 2 style method that enables the AGHF initial guess to violate the constraints while still generating feasible solutions rapidly;
3. This paper describes a computationally tractable formulation to enforce input constraints in the AGHF without increasing the system's state dimension
</div><!-- END METHOD -->

<!-- START RESULTS -->
<div markdown="1" class="content-block grey justify">

# [Results](#simulation-results)
## BLAZE Solve Time Comparison
This section shows the solve of BLAZE against comparison methods RAPTOR, Crocoddyl and Aligator.
In each of these trials input and joint limits are enforced, but there are no obstacle avoidance constraints.
The results show the time each method takes to solve a fixed time swing up problem for a 1-, 2-, 3-, 4-, and 5-link pendulum model (N = 1 to 5), a fixed time and final state specified trajectory optimization for the Kinova arm (N = 7), a bimanual manipulator system comprised of two Kinova arms (N = 14) and a trimanual manipulator system comprised of three Kinova arms (N = 21).

![link_construction](./assets/BLAZE_scalability_N1-21.png)
{: style="width:90%; margin:0 auto; display:block;" }

The bar plot above compares the mean solve times for the four different trajectory generation algorithms: BLAZE, RAPTOR, Crocoddyl and Aligator.
Each experiment was run ten times. Overall, we see that BLAZE shows better empirical scalability and solve times than the other methods as the system dimension increases.

## Kinova Simulation Obstacle Avoidance Results
The following videos demonstrate the various trajectories generated by BLAZE for the 7DOF Kinova Gen3 to navigate from the start configuration to the goal configuration while avoiding obstacles (red).
In each scenario, the initial guess trajectory is shown with its start/end in shades of orange, while the optimized solution trajectory is shown with its start/end in shades of green.
In each video the different color shades illustrate the evolution in time where the start configuration is in the darkest shade and the end configuration in the lightest. 
These initial guess trajectories collide with obstacles along their path, but BLAZE is able to push these initial guesses out of collision and generate the optimal collision-free solutions.
All of these collision-free trajectories were generated in &le; 3 seconds

<!-- START KINOVA CONS VIDEOS -->
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;">Initial Guess Trajectory</p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_16_sim_iso_iniguess.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;">Solution Trajectory</p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_16_sim_iso_solution.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>

<!-- Second batch of videos--> 
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;"> </p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_4_sim_front_iniguess.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;"> </p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_4_sim_front_solution.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>


<!-- Third batch of videos--> 
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;"> </p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_18_sim_iso_iniguess.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <p style="text-align: center; margin-top: 5px;"> </p>
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_18_sim_iso_solution.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>


## Kinova Hardware Obstacle Avoidance

<!-- Second batch of videos--> 
<p style="text-align: center; margin-top: 5px; font-size: 20px"> Initial Guess </p>
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_21_hardware_side_iniguess.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_21_hardware_front_iniguess.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>

<!-- Second batch of videos--> 
<p style="text-align: center; margin-top: 5px; font-size: 20px"> Solution on Hardware </p>
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 100%; max-width: 100%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scenario_21_hardware.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>

<!-- Second batch of videos--> 
<p style="text-align: center; margin-top: 5px; font-size: 20px"> Initial Guess </p>
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scene_005_hardware_side.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
  <div class="video-item" style="flex: 1 1 49%; max-width: 49%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scene_005_hardware_front.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>

<p style="text-align: center; margin-top: 5px; font-size: 20px"> Solution on Hardware </p>
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 100%; max-width: 100%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/scene_5_hardware.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>

<p style="text-align: center; margin-top: 5px; font-size: 20px"> Solution on Hardware </p>
<div class="video-container" style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
  <div class="video-item" style="flex: 1 1 100%; max-width: 100%;">
    <video
      class="autoplay-in-frame"
      preload="none"
      disableremoteplayback
      disablepictureinpicture
      playsinline
      muted
      loop
      onclick="this.paused ? this.play() : this.pause();"
      style="display:block; width:100%; height:auto;">
      <source src="assets/hardware_bins.mp4" type="video/mp4">
      Your browser does not support this video.
    </video>
  </div>
</div>


</div><!-- END RESULTS -->

<div markdown="1" class="justify">
  

<div markdown="1" class="content-block grey justify">
  
# [Citation](#citation)

This project was developed in [Robotics and Optimization for Analysis of Human Motion (ROAHM) Lab](http://www.roahmlab.com/) at the University of Michigan - Ann Arbor.

<!-- ```bibtex
@ARTICLE{enninfulphlame2024,
  author={Adu, Challen Enninful and Chuquiure, César E. Ramos and Zhang, Bohao and Vasudevan, Ram},
  journal={IEEE Robotics and Automation Letters}, 
  title={Bring the Heat: Rapid Trajectory Optimization With Pseudospectral Techniques and the Affine Geometric Heat Flow Equation}, 
  year={2025},
  volume={10},
  number={4},
  pages={4148-4155},
  keywords={Heuristic algorithms;Heating systems;Robots;Trajectory optimization;Vectors;Partial differential equations;Dynamic programming;Planning;Optimal control;Faces;Optimization and optimal control;motion and path planning;integrated planning and control},
  doi={10.1109/LRA.2025.3547299}}

``` -->
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