# hello-mlsys

[中文](./README.md) | English

**Learn ML systems on AMD hardware: model the whole system, prove the model on real hardware, then design better systems together with AI.**

hello-mlsys is an 8-week community learning programme about *systems*, not single tools or single kernels. The unit of study is always the whole system: **model × hardware × configuration × workload**, judged against latency targets, cost and energy. Each week you predict how the system will behave, measure it on real AMD hardware (from a Ryzen AI laptop to Instinct cloud GPUs), calibrate your model against the measurement, and decide what to try next. In the second half you team with AI on fixed design targets, scored on AMD hardware.

> Status: curriculum design. Items marked **TBD** will be confirmed before the first cohort.

## Where hello-mlsys fits

Each project works at a different layer. hello-mlsys starts where the others stop.

| Project | Unit of study | Core question | How hello-mlsys uses it |
| --- | --- | --- | --- |
| [hello-rocm](https://github.com/datawhalechina/hello-rocm) | One model on one AMD device | "How do I install, deploy and fine-tune on AMD?" | Prerequisite for setup; we do not re-teach installation or deployment |
| [hello-gpu](https://github.com/datawhalechina/hello-gpu) | One kernel | "How do I make this operator fast on an AMD GPU?" | Optional background; we treat kernels as black boxes with measured efficiency |
| [MLSys·im](https://github.com/harvard-edge/cs249r_book/tree/dev/mlsysim) (CS249r) | Any system, analytically, with no hardware | "Which wall will this system hit, in theory?" | Our prediction engine; we calibrate it on AMD hardware |
| **hello-mlsys** | **A whole system on real AMD hardware** | **"Which design meets the SLA at the lowest cost and energy, and how do I prove it?"** | Connects prediction, measurement and design decisions |

### What hello-mlsys does not teach

To avoid repeating good material that already exists:

| Out of scope here | Go to |
| --- | --- |
| Installing ROCm, drivers, PyTorch; framework-by-framework deployment tutorials (vLLM, llama.cpp, Ollama, LM Studio, Lemonade) | [hello-rocm](https://github.com/datawhalechina/hello-rocm) 00-environment and 01-deploy |
| Fine-tuning recipes (LoRA scripts, multi-GPU training scripts) | [hello-rocm](https://github.com/datawhalechina/hello-rocm) 02-fine-tune |
| GPU architecture in depth, HIP and Triton programming, kernel-level profiling, writing and fusing operators, agent-driven kernel optimisation | [hello-gpu](https://github.com/datawhalechina/hello-gpu) |

Every lab uses stock runtimes (PyTorch, vLLM, llama.cpp and the AMD libraries) as they are. When a lab needs a skill from those projects, we link the exact chapter as a prerequisite rather than re-teaching it.

### What we borrow from MLSys·im, and what we add

MLSys·im is a first-principles analytical modelling framework built around the idea that every ML system hits a "wall". We adopt several of its teaching ideas directly:

- **The walls as a shared vocabulary.** Every diagnosis names the binding constraint: compute, memory bandwidth, memory capacity, communication, software overhead, queueing, cost or energy.
- **The Iron Law.** Performance factors into a few multiplicative terms (device count, peak rate, utilisation, scaling efficiency, goodput), and every optimisation moves exactly one of them.
- **Predict, then reveal.** Every exercise starts with a written prediction. The course is built around designed "aha moments" where intuition breaks.
- **Inverse modelling.** Start from the SLA and work back to the hardware and configuration it requires, instead of benchmarking everything.
- **Sensitivity analysis.** Change each parameter a little and see which one moves the result most. That parameter is the next knob to turn.
- **Economics and sustainability.** Cost per token, energy per token and carbon are design constraints, not an afterthought.

We go further in four ways:

| | MLSys·im | hello-mlsys |
| --- | --- | --- |
| **Ground truth** | Analytical; its own docs put well-calibrated cases at ±15–30%, and production serving can be 1.5–2× slower | Every prediction is checked on AMD hardware. You measure the gap and fit **AMD calibration profiles** (utilisation, bandwidth efficiency, fixed overheads) per hardware tier. Missing AMD devices are added to the hardware registry and offered upstream |
| **AMD-specific system questions** | Mostly datacenter GPUs | Questions that only show up on AMD's range: NPU vs. integrated GPU vs. CPU on one laptop chip, large CPU–GPU shared memory on Ryzen AI Max (capacity without bandwidth), and very large HBM on Instinct (big models on one GPU, no tensor parallelism needed) |
| **Real optimisation** | Analyse a given configuration | Actually change the system (precision, context, batching, speculative decoding, parallelism, placement, tier mix) and see the real gain or loss |
| **Human–AI teaming** | None | AI proposes system designs and next experiments; you set constraints, prune with the model, verify on hardware and decide. The decision log is graded |

In short: hello-rocm teaches you to **run** a model on AMD, hello-gpu teaches you to make **one kernel** fast, MLSys·im teaches you to **predict** a system, and hello-mlsys teaches you to **design and prove a whole system**, together with AI, on AMD hardware.

## What you will learn

By the end of the programme, you will be able to:

1. **Predict.** Estimate the latency, throughput, memory, cost and energy of an ML system with back-of-envelope arithmetic and a simulator, before running it.
2. **Calibrate.** Measure the same system on AMD hardware, attribute where the time went, and fit a calibration profile so your next prediction is more accurate.
3. **Diagnose.** Name the wall a system is hitting (compute, bandwidth, capacity, communication, overhead, queueing, cost or energy) on anything from a laptop to a multi-GPU node.
4. **Design backwards.** Start from an SLA, a budget and an energy limit, work out what hardware and configuration they require, and prune designs that cannot work.
5. **Lead an AI teammate.** Judge an AI's diagnosis or design against evidence, catch it gaming a metric, and justify every decision in writing.

## Who it is for

- Students and engineers who can already run a model with PyTorch and want to reason about whole systems rather than single kernels.
- **Prerequisites:** Python, PyTorch basics, basic Linux, reading English documentation. You should be able to run an LLM on an AMD device (or complete the hello-rocm environment chapter first). **No kernel programming is needed.**
- **No GPU needed:** AMD cloud access is provided.
- **Time:** about 6–8 hours a week in Weeks 1–5, about 10 hours a week in Weeks 6–8.

## How it works

Every week follows the same loop:

```
Predict (napkin math + simulator) → Measure (AMD hardware) → Calibrate (fix the model) → Decide the next experiment
```

**Part A: model and calibrate (Weeks 1–5, individual).** Each week has readings, one hands-on lab, and a set of *case cards*. A case card shows a real system run: the prediction, the AMD measurement, a timeline, and an AI's diagnosis, which is sometimes wrong. You name the wall, decide whether the AI is right, and predict what a change will do before the result is revealed. Across Part A you build a calibrated model of your AMD hardware, which you then use in Part B.

**Part B: design with AI (Weeks 6–8, individual).** You work one-on-one with AI on three fixed design targets. Every project is your own. For each target you:

1. **Frame** the goal, constraints and quality floor yourselves.
2. **Design backwards:** derive what the SLA and budget require before trying anything.
3. **Plan** with AI, and choose, revise or reject its proposals.
4. **Prune** infeasible designs with your calibrated model.
5. **Measure** the remaining candidates on AMD hardware, writing down your prediction first.
6. **Evolve:** pick the next experiment and record why.

Every decision goes into a decision log, which is graded alongside your result and your prediction accuracy.

## Who does what

| Party | Role |
| --- | --- |
| **AMD** | Ground truth. Every case you study and every score you earn comes from AMD hardware. AMD also provides the ROCm stack, cloud credits, engineer office hours and showcase judges. |
| **Simulator (MLSys·im)** | Fast predictions, sensitivity analysis, and a way to rule out designs that cannot work. |
| **[Lumid](https://lum.id)** | The lab environment: case cards, measurement workflows, calibration tracking, AI planning and coding, suggested next experiments, and the decision log. |
| **You** | Frame the goal, judge the evidence, calibrate the model, verify results, and decide. |

## AMD hardware tiers

| Tier | Hardware | System questions it raises |
| --- | --- | --- |
| T1 | Ryzen AI PC (NPU + integrated GPU + CPU) | Which engine should run which phase; energy per token; tight memory |
| T2 | Radeon discrete GPU | Consumer VRAM limits; what quantisation buys; single-GPU serving |
| T3 | Ryzen AI Max (large CPU–GPU shared memory) | Capacity without matching bandwidth: big models fit, but how fast do they decode? |
| T4 | Instinct MI300-class (cloud) | Large HBM capacity and bandwidth; one-GPU vs. multi-GPU designs; cost per token at scale |

No device? AMD cloud access covers every week. Supported hardware list: **TBD**.

## Curriculum

| Week | Topic | By the end of the week, you can… | You submit | AMD |
| --- | --- | --- | --- | --- |
| **Part A** | **Model and calibrate** | | | |
| 1 | Napkin math and the walls | Predict a model's prefill and decode speed on a laptop and in the cloud from datasheets alone, and explain the gap | Gap report; first calibration entry; case cards | T1, T4 |
| 2 | Where did the time go? Calibrating on AMD | Break a model's run time into compute, memory, overhead and idle, and fit a per-tier calibration profile | Calibration profile with held-out error; case cards | T1–T4 |
| 3 | Memory is a design decision | Predict what fits where for inference and training, and show how precision changes a design, not just its speed | "What fits where" matrix; training-memory prediction; case cards | T2, T3, T4 |
| 4 | Serving is a queueing system | Predict the throughput knee and p99 of a serving system, and derive what an SLA requires | p99 and knee report; case cards | T4 |
| 5 | Scale, cost and energy | Model communication, cost per token and energy per token, and find where adding GPUs stops helping | Scale-out and cost memo; **Part A gate** | T1, T4 |
| **Part B** | **Design with AI** | | | |
| 6 | Target 1: an on-device assistant under a memory and energy budget | Choose model, precision and engine placement to meet a latency target at the lowest energy | Best configuration per device; decision log | T1, T3 |
| 7 | Target 2: meet a serving SLA on one node | Derive requirements from the SLA, then find the configuration with the best goodput above a quality floor | Best configuration per tier; decision log | T4, T3 |
| 8 | Target 3: design a fleet; showcase | Design a multi-GPU fleet under SLA, cost, energy and failure constraints, and prove a slice of it | Goodput per dollar; design doc; showcase talk | T4 |

### Week-by-week content

<details>
<summary><b>Week 1: Napkin math and the walls</b></summary>

**Topics**
- The walls: compute, memory bandwidth, memory capacity, communication, software overhead, queueing, cost and energy
- The Iron Law: time = work ÷ (devices × peak rate × utilisation × scaling efficiency × goodput)
- Datasheet arithmetic: prefill is roughly 2 × parameters × tokens of work; decode speed at batch 1 is roughly bandwidth ÷ bytes of weights
- MLSys·im basics: the hardware and model registries, a first `solve`, and what the tool does and does not model
- Honest accuracy: the simulator is good at naming the bottleneck and comparing options, and weaker at absolute latency

**Readings**
- [mlsys-course, chapter 0: systems thinking](https://yuuinih.github.io/mlsys-course/modules/00-systems-thinking.html)
- [MLSys·im](https://github.com/harvard-edge/cs249r_book/tree/dev/mlsysim): getting started and the cheat sheet
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: introductory chapters
- Williams, Waterman and Patterson, "Roofline: An Insightful Visual Performance Model" (2009)

**Lab:** Using a stock runtime (setup: hello-rocm), run one small LLM on a personal device (T1 or T3) and on a cloud GPU (T4). Before each run, predict prefill and decode speed twice: once from the FLOPs ratio between the devices and once from the bandwidth ratio. Then measure.

**Aha moment:** The cloud GPU has far more compute than the laptop, but decode speed tracks the bandwidth ratio instead.

**Case cards:** Three introductory cards on reading a prediction against a measurement and naming the wall.

**Submit:** A one-page gap report (prediction, measurement, the three biggest reasons they differ) and your first calibration entry: measured ÷ predicted for each device.
</details>

<details>
<summary><b>Week 2: Where did the time go? Calibrating on AMD</b></summary>

**Topics**
- Attributing a whole model's run time: compute, memory traffic, host and framework overhead, launch gaps, idle
- Model-level utilisation: MFU (model FLOPs utilisation) and MBU (model bandwidth utilisation)
- Reading a model-level timeline with the PyTorch profiler. The goal is attribution, not kernel tuning; for kernel-level profiling see hello-gpu Part 1
- System-level remedies for overhead: batching, graph capture, `torch.compile`
- Calibration: fitting per-tier efficiency parameters, testing them on held-out runs, and reporting error honestly
- Sensitivity analysis: which parameter moves the prediction most

**Readings**
- [MLSys·im](https://github.com/harvard-edge/cs249r_book/tree/dev/mlsysim): understanding efficiency and empirical calibration docs
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: hardware acceleration and benchmarking
- PyTorch profiler documentation

**Lab:** Sweep batch size and sequence length for one model on two tiers. Fit a calibration profile per tier (MFU, MBU and a fixed per-step overhead) on half of the runs, and predict the other half. Add any AMD device missing from the simulator's hardware registry (for example Radeon or Ryzen AI Max), with sources for every number.

**Aha moment:** At small batch sizes on a fast GPU, the biggest slice of time is neither compute nor memory but overhead.

**Case cards:** An AI blames a slow run on one wall from a timeline. Decide which attributions are wrong and why.

**Submit:** Your calibration profile, its error on held-out runs, and a sensitivity table saying which parameter matters most on each tier.
</details>

<details>
<summary><b>Week 3: Memory is a design decision</b></summary>

**Topics**
- The inference memory model: weights, KV cache, activations, runtime workspace; how KV cache grows with context length and batch
- The training memory model: weights, gradients, optimiser state, activations; sharding (ZeRO and FSDP), activation checkpointing, LoRA
- Precision as an architecture decision: FP8 or INT4 can decide whether a model fits on one GPU, which changes the whole design, not just the speed
- Mixture-of-experts: total vs. active parameters, and what each costs in capacity and bandwidth
- AMD's memory range: small consumer VRAM, large shared memory on Ryzen AI Max, large HBM on Instinct; capacity and bandwidth as separate walls

**Readings**
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: model optimisation and training
- Kwon et al., "Efficient Memory Management for LLM Serving with PagedAttention" (2023)
- Rajbhandari et al., "ZeRO: Memory Optimizations Toward Training Trillion Parameter Models" (2020)
- [MLSys·im](https://github.com/harvard-edge/cs249r_book/tree/dev/mlsysim): the training-memory model and "How much memory does Llama 3 need?"

**Lab:** Build a "what fits where" matrix: three model sizes (one of them MoE) × three precisions × tiers T2, T3 and T4. For each cell, predict whether it fits, the largest batch at a fixed context, and decode speed, then measure. Then predict the peak memory of a small fine-tune with and without activation checkpointing and LoRA, and measure it.

**Aha moments:** A model that "fits" runs out of memory as soon as KV cache grows with concurrency. The largest model fits on Ryzen AI Max, but bandwidth decides whether it is usable.

**Case cards:** Out-of-memory failures and quantisation results. Judge the AI's explanation of each.

**Submit:** The completed matrix, predicted and measured, the training-memory prediction, and the biggest gaps explained.
</details>

<details>
<summary><b>Week 4: Serving is a queueing system</b></summary>

**Topics**
- SLOs: time to first token, time per output token, end-to-end latency; p50 vs. p99
- Prefill and decode as two different machines sharing one GPU; continuous batching and chunked prefill
- Queueing basics: utilisation, Little's law, why latency explodes near the "knee", and goodput under an SLO
- Prefix caching and speculative decoding as system trade-offs: hit rate, acceptance rate, and what each costs
- Inverse modelling: from a latency target back to the bandwidth, memory and concurrency it requires

**Readings**
- Yu et al., "Orca: A Distributed Serving System for Transformer-Based Generative Models" (2022)
- Agrawal et al., "Taming Throughput-Latency Tradeoff in LLM Inference with Sarathi-Serve" (2024)
- Leviathan et al., "Fast Inference from Transformers via Speculative Decoding" (2023)
- [MLSys·im](https://github.com/harvard-edge/cs249r_book/tree/dev/mlsysim): the serving-capacity model

**Lab:** Load-test a single-GPU endpoint at rising arrival rates. Before measuring, predict the knee from your calibrated service time and a queueing model. Then pick one knob (chunked prefill, prefix caching or speculative decoding), predict its effect on p99 and goodput, and measure it.

**Aha moment:** At 80% utilisation the system looks healthy on average while p99 has already failed the SLO.

**Case cards:** "Throughput went up, p99 got worse" and similar cases. Judge three fixes the AI proposes.

**Submit:** A report with the predicted and measured knee, p99 curves, and the result of your chosen knob.
</details>

<details>
<summary><b>Week 5: Scale, cost and energy</b></summary>

**Topics**
- Collective communication: the α–β cost model for all-reduce; links inside a node vs. across nodes
- Tensor, pipeline, data and expert parallelism as trade-offs; hot experts in MoE; separating prefill from decode
- Scaling efficiency: the point beyond which more GPUs make a job slower
- Cost: price per GPU-hour, cost per million tokens, and why utilisation dominates it
- Energy and carbon: joules per token on a laptop NPU, integrated GPU and cloud GPU; grid carbon intensity
- Reliability at scale: failure rates, checkpoint intervals and goodput

**Readings**
- [mlsysbook.ai](https://mlsysbook.ai) Vol II: collective communication, inference at scale, sustainable AI
- Shoeybi et al., "Megatron-LM" (2019); Zhong et al., "DistServe" (2024)
- RCCL documentation and rccl-tests

**Lab:** Sweep all-reduce message sizes with rccl-tests on a multi-GPU node and fit α and β. Use them to predict at what GPU count tensor parallelism stops helping for one model, then check two points. Measure joules per token for the same small model on the T1 NPU, the T1 integrated GPU and T4, and compute cost per million tokens on T4.

**Aha moments:** More GPUs made p99 worse. The laptop NPU is the slowest option but uses the least energy per token.

**Case cards:** Scale-out, cost and energy cases. Judge the AI's recommendations.

**Submit:** A one-page scale-out and cost memo. Then take the Part A gate.
</details>

<details>
<summary><b>Week 6: Target 1, an on-device assistant under a memory and energy budget</b></summary>

**Brief:** A fixed on-device assistant workload trace, a latency target (time to first token and time per output token, **TBD**), a memory cap that leaves room for other applications, and a quality floor on a fixed evaluation set (**TBD**). The objective is the lowest energy per request.

**You may change:** model and size, precision, context length, which engine (NPU, integrated GPU, CPU) runs prefill and decode where the runtime allows, and speculative decoding with a small draft model.

**You may not change:** the trace, the latency target, the memory cap or the evaluation set.

**Your job:** Find the best configuration on T1 and T3 and explain why they differ. Record your energy prediction before every measurement.

**Readings:** Revisit Weeks 1, 3 and 5. For runtime setup on Ryzen AI, see hello-rocm 00-environment.

**Submit:** Your best configuration for each device, its measured energy per request and latency, and your decision log.
</details>

<details>
<summary><b>Week 7: Target 2, meet a serving SLA on one node</b></summary>

**Brief:** A fixed workload trace, an SLA of p99 time to first token < **X ms (TBD)** and p99 time per output token < **Y ms (TBD)**, and a quality floor on a fixed evaluation set (**TBD**).

**You may change:** model size, precision, context length, prefix caching, chunked prefill, concurrency, KV-cache memory fraction, speculative decoding.

**You may not change:** the trace, the SLA, or the evaluation set.

**Your job:** Start with inverse modelling: derive what the SLA requires and prune with your calibrated model before touching hardware. Then find the best configuration on T4 and T3 and explain why they differ. The quality floor rules out the easy win of the smallest model at the lowest precision.

**Readings:** Revisit Weeks 3–4.

**Submit:** Your best configuration for each tier, its goodput at the SLA, how close your predictions were, and the decision log.
</details>

<details>
<summary><b>Week 8: Target 3, design a fleet; showcase</b></summary>

**Brief:** A heavier trace under the same SLA, a cost budget, an energy budget, and a requirement to keep serving (at reduced goodput) when one GPU fails.

**Design choices:** number and type of GPUs, replicas vs. tensor parallelism, separating prefill from decode, request routing, redundancy.

**Your job:** Design the full fleet with your calibrated model, then prove it by measuring a representative slice on T4 and showing the prediction for that slice was accurate. Name the binding constraint that stops further improvement.

**Readings:** Revisit Week 5; [mlsysbook.ai](https://mlsysbook.ai) Vol II: inference at scale and sustainable AI.

**Submit:** Goodput at the SLA per dollar and per joule, a two-page design document, the decision log, and a short showcase talk.
</details>

## Assessment

**Part A.** Case cards are scored automatically. Lab write-ups are checked for a clear prediction, a measurement, and an explanation of the gap. Calibration profiles are scored on held-out prediction error. The **Part A gate** in Week 5 is a set of unseen case cards, taken without AI help; you need to pass it to join Part B.

**Part B.** Each target is scored three ways: the result measured on AMD hardware, the accuracy of the predictions you wrote down before each run, and your decision log. A strong log shows that you:

- set the goal, constraints and quality floor yourselves, before asking the AI for plans;
- derived requirements from the SLA and pruned with the model before measuring;
- accepted or overrode AI proposals based on evidence, and caught it gaming the metric;
- checked quality before claiming any speedup or saving;
- tracked how accurate your predictions were, and explained the misses.

**Using AI.** AI help is encouraged everywhere except the Part A gate. You must be able to explain and verify everything you submit.

### To complete the programme

- Case cards for at least 4 of the 5 Part A weeks, and a pass on the Part A gate.
- At least 2 of the 3 Part B targets, each with a decision log.
- A review of one other participant's decision log.

**You leave with** a portfolio of gap reports, calibration profiles and decision logs, a place on a public leaderboard measured on AMD hardware, and a completion certificate. Top participants present at the showcase and are eligible for AMD prizes.

## Getting started

1. Make sure you can run an LLM on an AMD device by following the [hello-rocm environment guide](https://github.com/datawhalechina/hello-rocm), or use AMD cloud access if you have no supported device.
2. Install the simulator: `pip install mlsysim`.
3. Create a Lumid account (**link TBD**) and open the Week 1 lab.
4. Join the cohort group (**TBD**).

## Project structure (planned)

```
hello-mlsys/
├── README.md
├── README_en.md
├── docs/
│   ├── week01-napkin-math-walls/
│   ├── week02-time-attribution-calibration/
│   ├── week03-memory-design/
│   ├── week04-serving-queueing/
│   ├── week05-scale-cost-energy/
│   ├── week06-target-on-device/
│   ├── week07-target-sla-serving/
│   └── week08-target-fleet/
├── labs/            # lab starter code and tests
├── calibration/     # AMD calibration profiles and hardware registry entries for the simulator
├── case-cards/      # case card index (cards are served through Lumid)
├── targets/         # Part B briefs, traces, evaluation sets, scoring
└── assets/
```

## Roadmap

- [x] Curriculum design
- [ ] Hardware whitelist and cloud credits confirmed with AMD
- [ ] AMD hardware entries and first calibration profiles for T1–T4, offered upstream to MLSys·im
- [ ] Week 1–5 labs and case cards (about 10 cards per week)
- [ ] Part B targets: traces, SLA thresholds, energy measurement method, quality floor
- [ ] Lumid lab apps and leaderboard
- [ ] Pilot cohort

## Contributing

Issues and pull requests are welcome, especially new case cards drawn from real AMD runs, calibration data from hardware not yet covered, and lab improvements.

## Acknowledgements

hello-mlsys builds on [CS249r / MLSys·im](https://github.com/harvard-edge/cs249r_book), [mlsys-course](https://github.com/YuuinIH/mlsys-course), and the [Machine Learning Systems course at NUS](https://mlsys.io/MLsys_25Sem2.html), and is designed to sit alongside [hello-rocm](https://github.com/datawhalechina/hello-rocm) and [hello-gpu](https://github.com/datawhalechina/hello-gpu). Hardware and cloud access are provided by AMD.

## License

**TBD**
