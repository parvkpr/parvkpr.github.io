---
layout: about
title: about
permalink: /
subtitle: 413 TCS Hall, Pittsburgh, PA #<a href='#'>Affiliations</a>. Address. Contacts. Moto. Etc.

profile:
  align: right
  image: prof_pic.jpg
  # address: >
  #   # <p>555 your office number</p>
  #   # <p>123 your address street</p>
  #   # <p>Your City, State 12345</p>

news: true  # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

Hi! I am a second-year graduate student at the [Institute for Software Research](https://www.isri.cmu.edu/), [Carnegie Mellon University](https://www.cmu.edu/) currently pursuing a Ph.D. in Software Engineering. 

I am broadly interested in assuring safety in robots and autonomy. My current research focuses on: 
1. Robustness of reinforcement learning agents
2. Safe planning and control for aerial robots 
3. Runtime monitoring for cyber-physical systems. 

I am advised by [Dr. Eunsuk Kang](https://eskang.github.io/) and [Dr. Sebastian Scherer](https://www.ri.cmu.edu/ri-faculty/sebastian-scherer/). I am part of [AirLab](http://theairlab.org/) and [SoDA](https://cmu-soda.github.io/#/). I also work with [Dr. Ding Zhao](https://www.meche.engineering.cmu.edu/directory/bios/zhao-ding.html)'s [SafeAI lab](https://safeai-lab.github.io/). 

Prior to joining CMU, I worked with [Dr. Jyotirmoy Deshmukh](https://jdeshmukh.github.io/)'s [CPS VIDA](https://cps-vida.github.io/) lab at [University of Southern California](https://www.usc.edu/).


TLDR; <font size="+2"><a href='/assets/pdf/Parv Kapoor.pdf'>Resume</a></font>

### Contact me

[parvk@cs.cmu.edu](mailto:parvk@cs.cmu.edu)

### Selected Projects  
- - - -

##### <span style="color:Maroon;font-family:Georgia;"> [On the Robustness of Reinforcement Learning agents](/blog/2022/proj6/) </span>
<span style="color:#29465B;font-family:serif;">Changjian Zhang</span>, <span style="color:#29465B;font-family:serif;font-weight:bold;"> Parv Kapoor</span><span style="color:#29465B;font-family:serif;">, Romulo Meira Goes, David Garlan, Eunsuk Kang </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/robust.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"> In this work, we use software engineering techniques to evaluate robustness of reinforcement learning agents in the face of environmental deviations. We use logical falsification techniques to develop insights about a trained policy's robustness. This is an ongoing project and a paper on our findings is due for submission. Please reach out to me directly for a working draft!
</div>

</div>

- - - -
    

##### <span style="color:Maroon;font-family:Georgia;"> [Trust elicitation and restoration in human robot interaction](/blog/2022/proj6/) </span>
<span style="color:#29465B;font-family:serif;font-weight:bold;"> Parv Kapoor</span><span style="color:#29465B;font-family:serif;">, Angela Chen, Simon Chu, Henny Admoni </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/hript2.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"> <font size="+2"><a href='/assets/pdf/HRIFINAL.pdf'>Report</a> </font>  <br> In this work, We investigate the impact of customisation and perspective on perceived trust in an assistive robotics context. We provide different levels of customization possibilities over a baseline reinforcement learning policy trained using proximal policy optimization. Our user study findings indicate that increased levels of customization was associated with higher trust and comfort perceptions. The assistive robot design process can benefit significantly from our insights for designing trustworthy and customized robots. 

</div>

</div>

- - - -
    

##### <span style="color:Maroon;font-family:Georgia;"> [Safe and Seamless Operation of Manned and Unmanned Aircraft in Shared Airspace](/blog/2022/proj1/) </span>
<span style="color:#29465B;font-family:serif;font-weight:bold;"> Parv Kapoor</span><span style="color:#29465B;font-family:serif;">, Jay Patrikar, Sebastian Scherer, Jean Oh </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/fig4.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"> <font size="+2"><a href='/assets/pdf/challenges_in_shared_airspace.pdf'>Paper</a>  <a href='https://www.cs.cmu.edu/news/2022/ai-pilot'>Article</a> </font>  <br>We defined a new angular rate based control barrier function for safe collision avoidance of autonomous aircrafts. We evaluate our method on a realistic flight simulator with a human pilot acting as an adversary. 
</div>

</div>

- - - -


##### <span style="color:Maroon;font-family:Georgia;"> [Follow The Rules: Online Signal Temporal Logic Tree Search for Guided Imitation Learning in Stochastic Domains](/blog/2022/proj2/) </span>
<span style="color:#29465B;font-family:serif;"> Jasmine Aloor, Jay Patrikar,</span> <span style="color:#29465B;font-family:serif;font-weight:bold;">Parv Kapoor</span><span style="color:#29465B;font-family:serif;">, Sebastian Scherer, Jean Oh </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/fig1_newspecv2.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"><font size="+2"><a href='https://arxiv.org/abs/2209.13737'> Paper </a>  <a href='https://github.com/castacks/mcts-stl-planning'>  Code  </a>  <a href='https://www.youtube.com/watch?v=fiFCwc57MQs'>  Video </a>  </font>  <br> This work uses Monte Carlo Tree Search (MCTS) as a means of integrating STL specification into a vanilla LfD policy to improve constraint satisfaction. We propose augmenting the MCTS
heuristic with STL robustness values to bias the tree search towards branches with higher constraint satisfaction. 
<br>

</div>

</div>



- - - -

##### <span style="color:Maroon;font-family:Georgia;"> [Decomposition for Incremental Behavior Building from STL objectives ](/blog/2022/proj3/) </span>
<span style="color:#29465B;font-family:serif;font-weight:bold;"> Parv Kapoor</span>,<span style="color:#29465B;font-family:serif;"> Eunsuk Kang, Emma Shedden, Romulo Meira Goes </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/dibbs.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"> We define a novel Signal Temporal Logic decomposition scheme for Task and Motion Planning for robots with holonomic constraints. We also propose a novel Monte Carlo Tree Search method for planning over decomposed specifications. We will be submitting this work soon so please reach out for a working draft!
</div>
</div>
- - - -
##### <span style="color:Maroon;font-family:Georgia;"> [Model based Reinforcement Learning from STL specifications](/blog/2020/proj5/) </span>
<span style="color:#29465B;font-family:serif;font-weight:bold;"> Parv Kapoor</span>,<span style="color:#29465B;font-family:serif;"> Anand Balakrishnan, Jyotirmoy Deshmukh </span>
<br>
<div class="row justify-content-sm-center">
  <div class="col-sm">
        {% include figure.html path="assets/img/fetchreach.png" title="example image" class="img-fluid rounded z-depth-1"%}
    </div>

<div class="col-sm" style="text-align: justify"> <font size="+2"><a href='https://arxiv.org/abs/2011.04950'>Paper</a>  </font>  <br>  We use STL
specifications in conjunction with model-based learning to design
model predictive controllers that try to optimize the satisfaction
of the STL specification over a finite time horizon.
<br>

</div>
</div>

