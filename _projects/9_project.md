---
layout: page
title: RL Robustness Analysis
description: Analysing robustness of safe reinforcement learning policies and control agents in the face of environmental deviations such as steering, friction, and sensor noise.
img: assets/img/rlrob.gif
importance: 8
category: work
selected: true
venue: FM 2024
arxiv: https://arxiv.org/abs/2406.17066
---

<p style="text-align: center; color: #e07b39; font-weight: bold; font-size: 1.3em;">Formal Methods (FM) 2024</p>

<a href="https://arxiv.org/abs/2406.17066" class="btn btn-primary mt-3" target="_blank" rel="noopener noreferrer">
📄 Read the Paper
</a>

<section class="mt-4">
<p>
Safe reinforcement learning policies are often evaluated under idealized simulation conditions, but real-world deployment exposes them to environmental deviations — variations in steering, friction, and sensor noise that can cause policy failures. This work designs stochastic optimization methods to systematically analyze the robustness of RL policies and control agents against such deviations in cyber-physical systems.
</p>
<p>
We evaluate policies for autonomous driving under actuation and environmental perturbations, in collaboration with Toyota Motor North America, and identify failure cases that are missed by standard evaluation protocols.
</p>
</section>

<section class="mt-4">
  <div class="row mt-4 justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
      {% include figure.html path="assets/img/rlrob.gif" title="RL Robustness Analysis" class="img-fluid rounded z-depth-1" %}
    </div>
  </div>
</section>
