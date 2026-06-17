<br /><br /><br />

## Abstract

End-to-end autonomous driving models are typically trained on offline human-demonstration datasets that provide limited state coverage and often no closed-loop feedback, making them prone to compounding errors when deployed in closed-loop and brittle to long-tail agent interactions. To overcome these limitations, we propose an alternative strategy for training end-to-end driving models: large-scale self-play directly from pixels in simulation. While prior self-play approaches have shown promising transfer to real-world driving, they typically assume vectorized Bird’s-Eye-View (BEV) observations that are incompatible with end-to-end policies operating directly on sensor observations. To this end, we introduce Gigapixel, a high-throughput batched driving simulator with perspective rendering, enabling scalable self-play directly from pixel observations. Rather than targeting compute-costly photorealistic sensor simulation, Gigapixel renders a simplified bounding-box world that preserves essential scene structure while achieving throughput at 50k agent steps per second. Since direct pixel-space self-play RL is prohibitively sample-inefficient at end-to-end model scale, we propose self-play DAgger training: we train pixel-based policies in self-play via on-policy distillation from a privileged RL teacher. To bridge the sim-to-real gap, we subsequently transfer the self-play trained policies to real-world sensor data through lightweight perception adaptation. Policies trained in Gigapixel and adapted to real-world sensor data achieve competitive performance on the HUGSIM and NAVSIM-v2 benchmarks without human trajectory supervision. Moreover, scaling self-play training yields proportional gains in policy performance, establishing self-play as a practical and scalable strategy for training end-to-end models.

## Citation

```bibtex
@article{rowe2026gigapixel,
  title   = {Scaling Self-Play for End-to-End Driving},
  author  = {Rowe, Luke and Girgis, Roger and de Schaetzen, Rodrigue and Cornelisse, Daphne and Grandhi, Alaap and Heide, Felix and Vinitsky, Eugene and Pal, Christopher and Paull, Liam},
  journal = {arXiv preprint},
  year    = {2026}
}
```