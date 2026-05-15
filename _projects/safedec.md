---
layout: page
title: Constrained Decoding for Robot Foundation Models
description: Enforces safety specifications expressed as Signal Temporal Logic (STL) formulas on transformer-based robot foundation models at inference time — no retraining required.
img: assets/img/top_down_cd.gif
importance: 1
category: work
selected: true
venue: ICML 2026
arxiv: https://arxiv.org/abs/2509.01728
redirect: /projects/safedec/
website: https://constrained-robot-fms.github.io/
# redirect: https://arxiv.org/abs/2311.07462
# skills: [RL,STL, SE4AI, Robustness]
---
<!-- 
<header class="mb-4">
  <h1 class="display-4">SafeDec: Constrained Decoding for Safer Generalist Robot Policies</h1>
  <p class="lead">We adapt constrained decoding from language models to robot foundation models—enforcing Signal Temporal Logic (STL) specifications at inference-time, without retraining.</p>
</header> -->
<p style="text-align: center; color: #e07b39; font-weight: bold; font-size: 1.3em;">International Conference on Machine Learning (ICML) 2026</p>

<a href="https://www.arxiv.org/abs/2509.01728" 
    class="btn btn-primary mt-3" 
    target="_blank" 
    rel="noopener noreferrer">
📄 Read the Paper
</a>
<!-- Problem framing + TL;DR combined -->
<div class="alert alert-primary mt-3" role="alert">
  <p style="margin-bottom: 0.6rem;">
    <strong>Key Question:</strong>
    How can we enforce contextual user rules such as
     <em>“don’t approach the bedroom if you have food in hand”</em>  for robot foundation models, even when the models weren’t explicitly trained for them?
  </p>
  <p style="margin-bottom: 0;">
    <strong>Our Approach:</strong>
    A constrained decoding method for robot foundation models that enforces contextual safety rules at inference time without retraining.
  </p>
</div>

<section id="motivation" class="mt-4">
  <!-- <h2>Why SafeDec?</h2> -->
<p>
  Recent Robot Foundation Models (RFMs) such as 
  <a href="https://spoc-robot.github.io/" target="_blank" rel="noopener">SPOC</a>, 
  <a href="https://robot-flare.github.io/" target="_blank" rel="noopener">FLaRe</a>, 
  <a href="https://poliformer.allen.ai/" target="_blank" rel="noopener">PoliFormer</a>, and 
  <a href="https://openvla.github.io/" target="_blank" rel="noopener">OpenVLA</a> 
  map multimodal inputs (RGB, language, proprioception) directly to action sequences and generalize well across navigation and manipulation tasks. 
  Trained on large language-conditioned trajectory datasets, they achieve strong zero-shot transfer on diverse goals such as object-centric tasks ("find a mug"), 
  spatial tasks ("visit all rooms"), and attribute-conditioned variants ("locate the chair closest to the refrigerator"). 
  While these models demonstrate robust real-world performance, they remain <b>purely data-driven and have no explicit notions of safety</b>. For instance, they cannot reliably enforce contextual constraints such as “avoid entering the bedroom while carrying food.” To deploy these policies out in the wild, we need methods beyond depending on implicit biases of pretraining data.
</p>
<!-- Embedded FLaRe demo video -->

<div class="d-flex flex-wrap justify-content-center align-items-start gap-4 mt-4 mb-4">

  <!-- Video 1: Apple -->
  <div style="flex: 1 1 45%; max-width: 720px; min-width: 340px;">
    <div style="position: relative; width: 95%; padding-bottom: 50%; height: 0; overflow: hidden; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
      <iframe
        src="https://www.youtube.com/embed/WjH7xYg03Bo"
        title="Find and Hold an Apple (from Fetch Task)"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border-radius: 8px;">
      </iframe>
    </div>
    <div class="caption text-center mt-2">
      <strong>Find and Hold an Apple</strong>
      (Video source: <a href="https://robot-flare.github.io/" target="_blank" rel="noopener">FLaRe</a>)
    </div>
  </div>

  <!-- Video 2: Mug -->
  <div style="flex: 1 1 45%; max-width: 720px; min-width: 340px;">
    <div style="position: relative; width: 95%; padding-bottom: 50%; height: 0; overflow: hidden; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
      <iframe
        src="https://www.youtube.com/embed/m4a85_nC3-k"
        title="Grasp a Mug (from Pickup Task)"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border-radius: 8px;">
      </iframe>
    </div>
    <div class="caption text-center mt-2">
      <strong>Grasp a Mug </strong>
       (Video source: <a href="https://robot-flare.github.io/" target="_blank" rel="noopener">FLaRe</a>)
    </div>
  </div>

</div>

<p>
  A possible way to enforce these rules could be through fine tuning a pretrained model on safe demonstrations. However, retraining is expensive and can’t ensure provable safety due to model stochasticity. To overcome this challenge, We need an <strong>inference-time</strong> approach to enforce safety requirements.
</p>
</section>

<section id="inspiration" class="mt-5">
  <h2>Background on structured outputs for LLMs</h2>

  <p>
     For <em>language models (LLMs)</em>, syntactic correctness such as following JSON schemas can be enforced at inference time without finetuning. <strong>Constrained decoding</strong> ensures generated sequences satisfy syntactic or structural rules without retraining. Early beam search based approaches would prune tokens that violate constraints <sup><a href="#ref-hokamp2017">[1]</a></sup>, while recent frameworks enable grammar- and program-aligned outputs via lightweight runtime control <sup><a href="#ref-willard2023">[2]</a>, <a href="#ref-beurer2023">[3]</a></sup>. Follow-on work extends this to regex schemas and context-free grammars <sup><a href="#ref-welleck2024">[4]</a>, <a href="#ref-park2024">[5]</a></sup>.
  </p>

  <p>
    In all these methods, the core principle is the same: <b>mask invalid next-tokens before they’re produced</b>, pruning probability mass of illegal continuations so the model outputs well-formed text (e.g., JSON) <sup><a href="#ref-willard2023">[2]</a>, <a href="#ref-welleck2024">[4]</a></sup>.
  </p>

<div class="row mt-4 justify-content-center">
  <!-- LMSYS-style constrained decoding -->
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.html path="assets/img/z_lmsys2.png"
       title="Token-level masking in LLM constrained decoding" class="img-fluid rounded z-depth-1" %}
  </div>

  <!-- Outlines / OpenAI JSON schema -->
  <div class="col-sm-6 mt-3 mt-md-0">
    {% include figure.html path="assets/img/z_outlines.png"
       title="Schema-aligned decoding (OpenAI Outlines)" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

<div class="caption text-center mt-2">
  <b>Left</b>: Masking invalid tokens during constrained decoding (source: <a href="https://lmsys.org/blog/2024-02-05-compressed-fsm/" target="_blank" rel="noopener">LMSYS blog</a>). 
  <b>Right</b>: JSON schema-aligned decoding using <a href="https://github.com/outlines-dev/outlines" target="_blank" rel="noopener">Outlines</a>.
</div>
  <p class="mt-2">
    <strong>Key insight:</strong> If we can prune invalid completions in LLMs, could we prune unsafe action sequences for RFMs? Since these RFMs are largely autoregressive transformers, can we apply similar ideas from constrained decoding? 
  </p>
</section>
  <p class="mt-5">
    <strong>The catch:</strong> A large class of robotics constraints are temporal and defined over state-based trajectories (e.g, avoid going through an unsafe region at all times). Additionally, most current RFMs output an action token. Thus, unlike LLMs, constraint checking can’t happen purely in token space—it needs <strong>forward simulation</strong> (a dynamics stepping function) to evaluate specification satisfaction as actions unfold <sup><a href="#ref-safedec-llm-contrast">[6]</a></sup>.
  </p>


<section id="llm-vs-rfm" class="mt-2">
  <!-- <h2>LLM Constrained Decoding vs. RFM Constrained Decoding</h2> -->
  <div class="row">
    <div class="col-md-6">
   <h5 class="mt-2 text-center">Large Language Models (LLMs)</h5>
      <ul>
        <li>Constraints defined on <strong>tokens</strong> (regex/grammar/JSON) that the model outputs <sup><a href="#ref-welleck2024">[4]</a>, <a href="#ref-park2024">[5]</a></sup></li>
        <li>Validity checkable on tokens during generation</li>
        <li>Some constraints can be captured using Finite State Automata</li>
      </ul>
    </div>
    <div class="col-md-6">
      <h5 class="mt-2 text-center">Robot Foundation Models (RFMs)</h5>
      <ul>
        <li>Constraints defined on <strong>state space</strong> while model outputs actions <sup><a href="#ref-safedec-llm-contrast"> [6]</a></sup></li>
        <li>Requires <strong>forward dynamics</strong> to evaluate outcomes <sup><a href="#ref-safedec-llm-contrast">[6]</a></sup></li>
        <li>Constraints can be captured using Temporal Logics </li>
      </ul>
    </div>
  </div>
</section>
  <p>
    <h2>Constrained decoding for RFMs</h2>
     We take the constrained-decoding principle and apply it to enforce safety specifications over state trajectories. Our safety rules are captured by Signal Temporal Logic (STL), a language defined over continuous signals from dynamical systems. We propose  <b>safety specification aligned decoding (SafeDec)</b> that simulates candidate actions with an approximate dynamics model and evaluates STL satisfaction in real time, directly inside the decoding loop <sup><a href="#ref-safedec-llm-contrast">[6]</a></sup>. Given an STL rule \( \varphi \) and a lightweight dynamics model \( f(x_t, a_t) \), each candidate next-action is <em>forecasted</em> and either <em>masked</em> (HCD) if it leads to a violation, or <em>reweighted</em> (RCD) by its satisfaction score to steer toward safer behavior—<em>inside</em> the decoder loop <sup><a href="#ref-safedec-hcd">[7]</a></sup>.
  </p>

  <div class="row mt-4 justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
      <img src="https://constrained-robot-fms.github.io/static/images/paper.png" class="img-fluid rounded z-depth-1" alt="SafeDec overview">
    </div>
  </div>
  <div class="caption">
    In LLMs, constraints are syntactic and local; in RFMs, constraints are temporal and depend on forward-simulated dynamics. SafeDec bridges the gap with STL-guided decoding <sup><a href="#ref-safedec-llm-contrast">[6]</a></sup>.
  </div>


  <p class="mt-2">

  </p>

<section id="how-it-works" class="mt-5">
  <p>
    SafeDec sits between the policy’s final logits and action selection. Given an STL spec <em>φ</em> and a lightweight dynamics model, it either <em>masks</em> or <em>reweights</em> action logits at each step so that the sampled action respects safety now—and in the near future.
  </p>

  <div class="row">
    <div class="col-md-6">
      <h5 class="mt-2">Hard Constrained Decoding (HCD)</h5>
      <p class="mb-1">
        If a candidate action’s predicted next state would violate <em>φ</em>, set its logit to −∞ (i.e., zero probability) before softmax. This yields <strong>provable compliance</strong> under the assumed dynamics.
      </p>
    </div>
    <div class="col-md-6">
      <h5 class="mt-2">Robustness Constrained Decoding (RCD)</h5>
      <p class="mb-1">
        Compute STL satisfaction score (<em>robustness</em>) ρ for each candidate’s predicted successor state and convert it to a weight that shifts the logits—boosting safer actions and suppressing risky ones (with tunable strength). This preserves task performance while greatly reducing violations.
      </p>
    </div>
  </div>

  <p class="mt-2">
    SafeDec is <strong>model-agnostic</strong>: it only needs (1) access to decoder logits and (2) an approximate dynamics function. STL evaluation is done efficiently via 
<a href="https://uw-ctrl.github.io/stlcg/" target="_blank" rel="noopener">STLCG++</a> for real time inference.
  </p>
</section>
<section id="showcase-1" class="mt-5">
  <h2>Sample Visualisations</h2>
  <!-- <p class="text-muted">
    Every project deserves a beautiful visual story. Below are flexible grid layouts you can adapt for SafeDec figures and demos.
  </p> -->

  <!-- First row -->
  <div class="row justify-content-center">
    <div class="col-md-6 mt-3 mt-md-0">
      {% include figure.html path="assets/img/top_cd.gif" 
         title="Base model behavior" 
         class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-md-6 mt-3 mt-md-0">
      {% include figure.html path="assets/img/top_down_cd.gif" 
         title="Safedec behavior" 
         class="img-fluid rounded z-depth-1" %}
    </div>
  </div>

  <div class="caption text-center mt-2">
    Each plot shows a bird's eye view of trajectories starting from the white dot under the instruction “find a sofa”. <b>Left:</b> The unconstrained model passes through two forbidden regions (red squares) on the way to the yellow sofa. <b>Right:</b> SafeDec modifies the trajectories to respect STL safety specifications while still reaching the goal.
  </div>

  <!-- Second row -->
<div class="row justify-content-center mt-4">
  <div class="col-12 mt-3">
    {% include figure.html path="assets/img/fpv_no_cd.gif" 
       title="FPV view of base model" 
       class="img-fluid rounded z-depth-1 w-100" %}
  </div>
  <div class="col-12 mt-3">
    {% include figure.html path="assets/img/fpv_cd.gif" 
       title="FPV view of safe model" 
       class="img-fluid rounded z-depth-1 w-100" %}
  </div>
</div>

  <div class="caption text-center mt-2">
    <b>Top:</b> FPV view of the base model violating the requirement.
    <b>Right:</b> FPV view of the model with SafeDec.
  </div>
</section>
<section id="results" class="mt-5">
  <h2>Results at a Glance</h2>
  <p>
    Evaluated on hundreds of procedurally generated AI2-THOR scenes with three SOTA policies (SPOC, FLaRe, PoliFormer), SafeDec enforced two invariant specs: <em>ϕ<sub>avoid</sub></em> (never enter forbidden zones) and <em>ϕ<sub>geofence</sub></em> (stay within allowed regions).
  </p>
  <ul>
    <li><strong>Unconstrained</strong>: Only ~68–78% geofence and ~72–77% avoid satisfaction.</li>
    <li><strong>HCD</strong>: ~100% spec satisfaction across models/specs, with a modest 5–10% success drop vs. baseline.</li>
    <li><strong>RCD</strong>: ~80–95% spec satisfaction with success rates close to unconstrained (often within 1–3%); best safety–performance trade-off overall.</li>
  </ul>
  <p>
    Compared against SafeVLA on SafetyChores, HCD reduces safety violations from <strong>0.205 → 0.015</strong>, achieving near-zero violations while maintaining competitive task success.
  </p>
  <div class="row mt-4 justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
      <img src="https://constrained-robot-fms.github.io/static/images/resu_safedec.png" class="img-fluid rounded z-depth-1" alt="SafeDec results">
    </div>
  </div>
</section>
<h3>Ablations</h3>

Since we assume a simple dynamics model (unicycle) for generating states from proposed actions, we evaluate the impact of noisy dynamics on final satisfaction. Additionally, we also sweep over the beta parameter for RCD, which controls how much specification satisfaction should be prioritized over task performance.
<section id="showcase-2" class="mt-5">
  <!-- 2/3 + 1/3 grid with vertical centering -->
  <div class="row justify-content-center align-items-center mt-4">
    
    <!-- Left: Larger image -->
    <div class="col-lg-6 col-md-5 mt-3 mt-md-0">
      {% include figure.html path="assets/img/ablation_dyn.png" 
         title="Ablation" 
         class="img-fluid rounded z-depth-1" %}
    </div>

    <!-- Right: Vertically centered smaller image -->
    <div class="col-lg-6 col-md-5 mt-3 mt-md-0 d-flex justify-content-center align-items-center">
      {% include figure.html path="assets/img/ablation_beta.png" 
         title="Ablation" 
         class="img-fluid rounded z-depth-1" %}
    </div>

  </div>

  <div class="caption text-center mt-2">
    <b>Left:</b> STL satisfaction (%) for HCD and RCD under baseline vs noisy dynamics across base models. 
    <b>Right:</b> Effect of β on success rate and safety satisfaction.
  </div>
</section>
  <p>
    SafeDec remains effective under dynamics noise; both HCD and RCD degrade gracefully. For our β ablation, we observe that as β increases for PoliFormer, both STL satisfaction and success rate improve in tandem until β = 10, suggesting that moderate regularization can actually aid policy execution. Beyond this, STL satisfaction continues to improve but at the cost of lower success rates. For Flare, larger β values improve STL satisfaction but reduce success rates. These results highlight that the influence of β is model-dependent but in general demonstrate that SafeDec provides a tunable mechanism to balance safety and performance objectives.
  </p>

<h3>Conclusion</h3>
<p>
In this post, we explored a constrained decoding framework that brings safety guarantees to large transformer-based robot policies. By enforcing safety specifications directly at inference time, our approach enables <b>real-time adaptation to new rules and environments without any retraining</b>.
</p>
<p>
We’re excited about how this line of work can make foundation models more reliable in the wild. Stay tuned as we share more results, open-source code, and demos soon!
</p>
<!-- 
<section id="llm-assets" class="mt-5">
  <h2>References &amp; Links</h2>
  <ul>
    <li>
      <strong>Guidance: A Language Model Control Framework</strong> — practical “guided generation” for LLMs (masking, constraints). GitHub: 
      <a href="https://github.com/guidance-ai/guidance">guidance-ai/guidance</a> .
    </li>
    <li>
      <strong>Efficient Guided Generation for Large Language Models</strong> — Willard &amp; Louf (2023). Discusses efficient constraint guidance during decoding .
    </li>
    <li>
      <strong>Prompting is Programming: A Query Language for LLMs</strong> — Beurer-Kellner et&nbsp;al. (PLDI&nbsp;2023). DOI: 
      <a href="https://doi.org/10.1145/3591300">10.1145/3591300</a> .
    </li>
    <li>
      <strong>From Decoding to Meta-Generation: Inference-Time Algorithms for LLMs</strong> — Welleck et&nbsp;al. (2024). arXiv: 
      <a href="https://arxiv.org/abs/2406.16838">2406.16838</a> .
    </li>
    <li>
      <strong>Grammar-Aligned Decoding</strong> — Park et&nbsp;al. (NeurIPS&nbsp;2024). Proceedings link: 
      <a href="https://proceedings.neurips.cc/paper_files/paper/2024/file/2bdc2267c3d7d01523e2e17ac0a754f3-Paper-Conference.pdf">NeurIPS 2024 paper</a> .
    </li>
    <li>
      <strong>DExperts: Decoding-Time Controlled Text Generation with Experts &amp; Anti-Experts</strong> — Liu et&nbsp;al. (2021). arXiv: 
      <a href="https://arxiv.org/abs/2105.03023">2105.03023</a> .
    </li>
  </ul>
</section> -->


<!-- <section id="limitations" class="mt-5">
  <h2>Limitations &amp; What’s Next</h2>
  <ul>
    <li><strong>Specs &amp; State</strong>: Today, specs are written over state variables and assume localization; authoring them can be burdensome.</li>
    <li><strong>Dynamics</strong>: We assume an approximate dynamics model; richer learned world models (e.g., MoSim) can improve fidelity.</li>
  </ul>
  <p>
    Looking ahead: open-world safety specs in embedding spaces, LLM-assisted spec generation, and tighter integration with learned dynamics.
  </p>
</section> -->

<section id="links" class="mt-5">
  <h2>Links</h2>
  <ul>
    <li>SafeDec: <a href="https://constrained-robot-fms.github.io/">Website</a> · <a href="https://arxiv.org/abs/2509.01728">arXiv</a></li>
    <li>STLCG++ (our STL engine): <a href="https://uw-ctrl.github.io/stlcg/">Website</a> · <a href="https://arxiv.org/abs/2501.04194">arXiv</a></li>
  </ul>
</section>
<section id="references" class="mt-5">
  <h2>References</h2>
  <ol>
    <li id="ref-hokamp2017">Hokamp, C., &amp; Liu, Q. (2017). <em>Lexically Constrained Decoding for Sequence Generation</em> (Grid Beam Search). ACL.</li>
    <li id="ref-willard2023">Willard, B., &amp; Louf, R. (2023). <em>Guidance: A Language Model Control Framework</em>. GitHub: <a href="https://github.com/guidance-ai/guidance">guidance-ai/guidance</a>.</li>
    <li id="ref-beurer2023">Beurer-Kellner, L., et&nbsp;al. (2023). <em>Prompting is Programming: A Query Language for LLMs</em>. PLDI. DOI: <a href="https://doi.org/10.1145/3591300">10.1145/3591300</a>.</li>
    <li id="ref-welleck2024">Welleck, S., et&nbsp;al. (2024). <em>From Decoding to Meta-Generation: Inference-Time Algorithms for LLMs</em>. arXiv: <a href="https://arxiv.org/abs/2406.16838">2406.16838</a>.</li>
    <li id="ref-park2024">Park, K., et&nbsp;al. (2024). <em>Grammar-Aligned Decoding</em>. NeurIPS. <a href="https://proceedings.neurips.cc/paper_files/paper/2024/file/2bdc2267c3d7d01523e2e17ac0a754f3-Paper-Conference.pdf">paper</a>.</li>
    <li id="ref-safedec-llm-contrast">SafeDec paper (this work): LLM vs RFM constraint contrast; need for dynamics stepping for spec checking (see §2.2 and §3.1).</li>
    <li id="ref-safedec-hcd">SafeDec paper (this work): HCD masking by setting logits to −∞ for spec-violating actions; RCD reweighting by STL robustness (see §3.2–§3.3).</li>
    <li id="ref-safedec-vision">SafeDec paper (this work): Model-agnostic, inference-time STL enforcement without retraining; overall vision (Intro &amp; §3).</li>
  </ol>
</section>
<hr class="my-5"/>