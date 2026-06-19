---
layout: default
---

<!-- ===================== OVERVIEW VIDEO ===================== -->
<section class="section section--alt" id="overview">
  <div class="container">
    <div class="titlewrap reveal">
      <h2>Video Overview</h2>
    </div>
    <div class="videoblock reveal" style="max-width:1000px">
      <video controls preload="none" playsinline poster="src/video_poster.jpg">
        <source src="src/video.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
    </div>
  </div>
</section>

<!-- ===================== METHOD / SYSTEM FIGURE ===================== -->
<section class="section" id="method">
  <div class="container">
    <div class="titlewrap reveal">
      <span class="eyebrow">Method</span>
      <h2>Self-Play for End-to-End Driving</h2>
    </div>

    <figure class="media media--pad fig fig--wide reveal">
      <img src="src/system_figure.png" alt="Three-stage self-play training pipeline: vectorized teacher, pixel-based student via self-play DAgger, and sim-to-real perception adaptation." />
      <figcaption><b>Overview.</b> <b>(a) Vectorized teacher.</b> A compact policy is trained with self-play reinforcement learning over fast vectorized BEV observations, yielding a robust, naturalistic driving policy. <b>(b) Pixel-based student (self-play DAgger).</b> The teacher is distilled into a pixel-based end-to-end policy inside Gigapixel. Every agent in the scene is controlled by the student, and at each visited state a <i>forked</i> parallel simulator rolls out the teacher to generate per-agent trajectory targets. <b>(c) Sim-to-real perception adaptation.</b> To deploy on real sensor observations, we freeze the planning head and finetune only the perception backbone on paired simulated&ndash;real observations, mapping real images into the latent representation the planning head already acts on.</figcaption>
    </figure>

    <div class="callout reveal">
      <b>Why self-play?</b> Behavior cloning learns from a fixed, narrow distribution of human logs and never sees the consequences of its own actions, so errors compound in closed-loop. In self-play, agents are paired with copies of themselves and learn through closed-loop interaction, which surfaces safety-critical interactions that are vanishingly rare in driving logs, and exposes the policy to the consequences of its own actions during training.
    </div>
  </div>
</section>

<!-- ===================== GIGAPIXEL SIMULATOR ===================== -->
<section class="section section--alt" id="simulator">
  <div class="container">
    <div class="titlewrap reveal">
      <span class="eyebrow">Simulator</span>
      <h2>The Gigapixel Renderer</h2>
      <p style="text-align:justify;hyphens:auto;-webkit-hyphens:auto">Gigapixel extends the PufferDrive batched simulator with the GPU-accelerated Madrona renderer, exposing ego-centric perspective views rather than only vectorized BEV features. It renders a deliberately simple bounding-box world (vehicles and static objects as cuboids, lane polylines as thin planar strips, and traffic lights as small spheres) preserving the scene geometry and interaction fidelity needed for planning while sustaining <strong>50k agent steps per second on a single GPU, scaling near-linearly with the number of GPUs</strong>.</p>
    </div>

    <!-- Rollouts -->
    <h3 class="reveal" style="text-align:center;margin-bottom:6px">Gigapixel Rollouts</h3>
    <p class="muted reveal" style="text-align:justify;hyphens:auto;-webkit-hyphens:auto;max-width:720px;margin:0 auto">The ego-centric pixel observations a policy receives during self-play, stitched across the forward camera views. Blue cuboids are surrounding agents, thin strips are lane polylines, and the small colored spheres are traffic lights.</p>
    <div class="grid2">
      <div class="rollout reveal">
        <video class="autoplay" muted loop playsinline preload="none" poster="src/gigapixel_rollouts/world_0013.jpg">
          <source src="src/gigapixel_rollouts/world_0013.mp4" type="video/mp4">
        </video>
      </div>
      <div class="rollout reveal">
        <video class="autoplay" muted loop playsinline preload="none" poster="src/gigapixel_rollouts/world_0030.jpg">
          <source src="src/gigapixel_rollouts/world_0030.mp4" type="video/mp4">
        </video>
      </div>
    </div>

    <!-- Rasterizer vs Ray Tracer -->
    <div class="split reveal" style="margin-top:64px">
      <div class="split__media">
        <figure class="media media--pad">
          <img src="src/rasterizer_ray_tracer.png" alt="Comparison of Gigapixel ray-traced and rasterized renderings of three scenes across resolutions from 64x64 to 512x512." />
        </figure>
      </div>
      <div>
        <h3>Two Rendering Backends</h3>
        <p>Gigapixel supports rasterized and ray-traced rendering. The rasterizer is faster, exploiting the simplicity of the primitives; the ray tracer trades throughput for higher visual fidelity, which is visible in the richer shading of the lower rows.</p>
      </div>
    </div>

    <!-- Throughput -->
    <div class="split split--rev reveal" style="margin-top:60px">
      <div class="split__media">
        <figure class="media media--pad">
          <img src="src/training_sps_vs_resolution.png" alt="Agent steps-per-second versus rendering resolution for Gigapixel rasterizer and ray tracer compared to HUGSIM and RAP renderers." />
        </figure>
      </div>
      <div>
        <h3>Throughput vs. Resolution</h3>
        <p>Agent steps per second (SPS) across rendering resolutions and policy architectures on one NVIDIA A100 GPU. <em>Render Only</em> isolates renderer throughput, with no policy forward or backward pass.</p>
        <ul>
          <li>The Gigapixel rasterizer is <strong>~1000&times; faster</strong> than the Gaussian-splatting HUGSIM renderer and <strong>~4000&times; faster</strong> than the CPU rasterizer RAP at 512&times;512.</li>
          <li>The gap between the rasterizer (Rast.) and ray tracer (RT) <strong>narrows as model complexity grows</strong> (CNN &rarr; DrivoR). This confirms that with a heavy end-to-end model, the renderer is no longer the throughput bottleneck.</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- ===================== RESULTS ===================== -->
<section class="section" id="results">
  <div class="container">
    <div class="titlewrap reveal">
      <span class="eyebrow">Results</span>
      <h2>Self-Play vs. Behavior Cloning</h2>
      <p style="text-align:justify;hyphens:auto;-webkit-hyphens:auto">We compare two pixel-based DrivoR policies driving closed-loop in photorealistic reconstructed real-world scenes. Both share the same architecture; only the training signal differs.</p>
    </div>

    <div class="legend reveal">
      <span><i class="g"></i>Self-Play Trained (trained in Gigapixel; adapted to real observations)</span>
      <span><i class="r"></i>BC Trained (human logs)</span>
      <span><i class="y"></i>Planned Trajectory</span>
    </div>

    <div class="cmpgrid">
      {% for i in (1..5) %}
      <div class="cmp reveal">
        <div class="cmp__head">
          <span class="cmp__num">Scenario {{ i }}</span>
          <span class="cmp__hint">Same scene &middot; two policies</span>
        </div>
        <div class="cmp__row">
          <span class="tag tag--good"><span class="d"></span>Self-Play Trained</span>
          <video class="autoplay" muted loop playsinline preload="none" poster="src/gigapixel_versus_drivor_{{ i }}/poster_gigapixel.jpg">
            <source src="src/gigapixel_versus_drivor_{{ i }}/web_gigapixel.mp4" type="video/mp4">
          </video>
        </div>
        <div class="cmp__row">
          <span class="tag tag--bad"><span class="d"></span>BC Trained</span>
          <video class="autoplay" muted loop playsinline preload="none" poster="src/gigapixel_versus_drivor_{{ i }}/poster_drivor.jpg">
            <source src="src/gigapixel_versus_drivor_{{ i }}/web_drivor.mp4" type="video/mp4">
          </video>
        </div>
      </div>
      {% endfor %}
    </div>

    <div class="callout reveal">
      <b>What to look for.</b> The self-play policy anticipates hazards: it reduces speed and plans smooth corrective maneuvers (e.g., yielding to a decelerating lead vehicle or steering back from the road edge). The behavior-cloned policy, never exposed to the consequences of its own actions, tends to keep a centered, high-velocity straight-ahead plan and fails in the rare safety-critical situations that precede a stop or a recovery (rear-ending a stopped vehicle or drifting off-road). On HUGSIM, the average collision velocity of the self-play policy is 1.95 m/s versus 5.27 m/s for behavior cloning, a 2.7&times; reduction.
    </div>

    <!-- Sample efficiency -->
    <div class="split reveal" style="margin-top:72px">
      <div class="split__media">
        <figure class="media media--pad">
          <img src="src/self-play-dagger-versus-rl.png" alt="Gigapixel Driving Score versus global step for self-play DAgger versus self-play RL, with the vectorized teacher as an upper reference." />
        </figure>
      </div>
      <div>
        <h3>Self-Play DAgger is More Sample-Efficient than Self-Play RL</h3>
        <p>Using a lightweight CNN policy (for tractable RL experimentation), we compare distilling a privileged teacher via self-play DAgger against training the pixel policy directly with self-play RL.</p>
        <ul>
          <li>Self-play DAgger surpasses a Gigapixel Driving Score of 60 in roughly <strong>3000&times; fewer steps</strong> than self-play RL.</li>
          <li>It quickly approaches the vectorized teacher's performance (dashed line, reached at 25B steps).</li>
          <li>This motivates distilling an RL teacher rather than training a large pixel policy with RL from scratch.</li>
        </ul>
      </div>
    </div>

    <!-- Scaling -->
    <div class="split split--rev reveal" style="margin-top:60px">
      <div class="split__media">
        <figure class="media media--pad">
          <img src="src/score-versus-steps.png" alt="Gigapixel Driving Score versus global step comparing self-play DAgger, single-agent DAgger, and behavior cloning." />
        </figure>
      </div>
      <div>
        <h3>Scaling Self-Play</h3>
        <p>Closed-loop performance of the DrivoR-Reg student as training experience scales in Gigapixel, across three end-to-end training strategies.</p>
        <ul>
          <li>Self-play DAgger <strong>improves consistently with scale</strong> and overtakes both single-agent DAgger and behavior cloning beyond 10M steps.</li>
          <li>Behavior cloning plateaus around 100M steps, as the student is never exposed to consequences of its own actions.</li>
          <li>Self-play's edge comes from two effects: <strong>every</strong> agent in a rollout contributes supervised data, and the co-evolving interactions span more diverse, safety-critical states.</li>
        </ul>
      </div>
    </div>

    <div class="callout reveal">
      <b>Headline results.</b> Gigapixel-DrivoR reaches <strong>state-of-the-art</strong> on the closed-loop HUGSIM benchmark (38.5 HD-Score, 50.1 route completion) and competitive performance on NAVSIM-v2, <strong>without human trajectory supervision</strong>. Scaling self-play yields proportional gains in policy performance.
    </div>
  </div>
</section>

<!-- ===================== CITATION ===================== -->
<section class="section section--alt" id="citation">
  <div class="container">
    <div class="titlewrap reveal">
      <span class="eyebrow">Citation</span>
      <h2>BibTeX</h2>
      <p>If you find this work useful, please consider citing it.</p>
    </div>
    <div class="cite-wrap reveal">
      <button class="copybtn" id="copy-bibtex" type="button">Copy</button>
      <pre id="bibtex-code"><span class="k">@article</span>{rowe2026gigapixel,
  title   = {Scaling Self-Play for End-to-End Driving},
  author  = {Rowe, Luke and Girgis, Roger and de Schaetzen, Rodrigue and
             Cornelisse, Daphne and Grandhi, Alaap and Heide, Felix and
             Vinitsky, Eugene and Pal, Christopher and Paull, Liam},
  journal = {arXiv preprint arXiv:2606.19641},
  year    = {2026}
}</pre>
    </div>
  </div>
</section>
