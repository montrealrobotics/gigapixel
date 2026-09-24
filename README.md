# Official Repository for Gigapixel

<p align="left">
<a href="https://arxiv.org/abs/2606.19641" alt="arXiv">
    <img src="https://img.shields.io/badge/arXiv-2606.19641-b31b1b.svg?style=flat" /></a>
<a href="https://montrealrobotics.ca/gigapixel" alt="webpage">
    <img src="https://img.shields.io/badge/Project Page-Gigapixel-blue" /></a>
</p>

> [**Scaling Self-Play for End-to-End Driving**](https://arxiv.org/abs/2606.19641)  <br>
> [Luke Rowe](https://rluke22.github.io)<sup>1,2</sup>, [Roger Girgis](https://mila.quebec/en/person/roger-girgis/)<sup>1,3,4</sup>, [Rodrigue de Schaetzen](https://rdesc.dev/)<sup>1,2,4</sup>, [Daphne Cornelisse](https://www.daphne-cornelisse.com/)<sup>5</sup>, [Alaap Grandhi](https://www.linkedin.com/in/alaap-grandhi/)<sup>1,6</sup>, [Felix Heide](https://www.cs.princeton.edu/~fheide/)<sup>4,7</sup>, [Eugene Vinitsky](https://www.eugenevinitsky.com/)<sup>5</sup>, [Christopher Pal](https://sites.google.com/view/christopher-pal)<sup>1,2,3</sup>, [Liam Paull](https://liampaull.ca/)<sup>1,2</sup>  <br>
> <sup>1</sup> Mila, <sup>2</sup> Université de Montréal, <sup>3</sup> Polytechnique Montréal, <sup>4</sup> Torc Robotics, <sup>5</sup> NYU Tandon School of Engineering, <sup>6</sup> McMaster University, <sup>7</sup> Princeton University <br>
> <br>
> CoRL 2026 <br>
>

All code, data, and models will be released by CoRL 2026. Please see the repository timeline below for more details.

Gigapixel is a high-throughput batched driving simulator with perspective rendering that enables large-scale self-play directly from pixels. We use Gigapixel to train end-to-end driving policies via large-scale self-play directly from pixels; these policies transfer to real-world sensor data through lightweight perception adaptation, without human trajectory supervision.

<!-- Tip: GitHub renders inline videos most reliably from drag-and-dropped uploads. To get an autoplaying inline player, edit this README on GitHub, drag assets/world_0013.mp4 into the editor, and replace the src below with the generated user-attachments URL. -->
<video src="https://github.com/user-attachments/assets/5c6521a7-1d36-475e-a6c7-6c7ac3162560" width="360"></video>

## Repository Timeline

- [ ] [ETA: 10/23/2026] Gigapixel renderer integrated into PufferDrive 2.0
- [ ] [ETA: 10/30/2026] NuPlan dataset extraction + link to preprocessed NuPlan data
- [ ] [ETA: 10/30/2026] Gigaflow teacher training and pre-trained checkpoints
- [ ] [ETA: 11/06/2026] Gigapixel DrivoR/DrivoR-Reg student training and pre-trained checkpoints
- [ ] [ETA: 11/13/2026] Scoring head training and perception adaptation training
- [ ] [ETA: 11/13/2026] Gigapixel-DrivoR/DrivoR-Reg perception-adapted checkpoints
- [ ] [ETA: 11/13/2026] Evaluation on Gigapixel, NAVSIM-v2 navhard, and HUGSIM

Table of Contents
=================
  * [Citation](#citation)

## Citation

```bibtex
@article{rowe2026gigapixel,
  title   = {Scaling Self-Play for End-to-End Driving},
  author  = {Rowe, Luke and Girgis, Roger and de Schaetzen, Rodrigue and Cornelisse, Daphne and Grandhi, Alaap and Heide, Felix and Vinitsky, Eugene and Pal, Christopher and Paull, Liam},
  journal = {arXiv preprint arXiv:2606.19641},
  year    = {2026}
}
```
