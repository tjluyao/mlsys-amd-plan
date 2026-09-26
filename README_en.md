# hello-mlsys

[中文](./README.md) | English

**Learn ML systems on AMD hardware: predict performance, measure it for real, then build better systems together with AI.**

hello-mlsys is an 8-week community learning programme. Each week you predict how an ML workload will perform, measure it on real AMD hardware (from a Ryzen AI laptop to Instinct cloud GPUs), and explain the gap. In the second half you team with AI on fixed engineering targets, scored on AMD hardware.

> Status: curriculum design. Items marked **TBD** will be confirmed before the first cohort.

## Why hello-mlsys

| Project | What it teaches | How hello-mlsys relates |
| --- | --- | --- |
| [hello-rocm](https://github.com/datawhalechina/hello-rocm) | Using AMD GPUs: setup, deployment, fine-tuning, HIP operators | We use it for environment setup and deployment readings |
| [hello-gpu](https://github.com/datawhalechina/hello-gpu) | GPU kernel programming on AMD | We use it for architecture and kernel readings |
| [MLSys·im](https://github.com/harvard-edge/cs249r_book) (CS249r) | Predicting system performance with a simulator | We use the simulator for predictions, then check them on real hardware |
| **hello-mlsys** | **Reasoning about whole ML systems, and leading AI to improve them** | Connects the three: predict, measure, explain, decide |

### How hello-mlsys differs from MLSys·im

MLSys·im is an excellent tool for predicting performance. hello-mlsys starts from it and goes further in four ways:

| | MLSys·im | hello-mlsys |
| --- | --- | --- |
| **Interactivity** | Change parameters, read the simulated result | Feedback at every step: predict, measure on AMD hardware, explain the gap, choose the next experiment. Case cards ask you to commit to a judgement before the result is revealed |
| **AI capabilities** | None | AI coding and planning built into Lumid. The AI offers diagnoses, designs and next-experiment suggestions, and you judge them |
| **Optimisation opportunities** | Analyse the bottleneck of a given configuration | Actually optimise: kernel code, quantisation, context length, concurrency, speculative decoding, parallelism and multi-GPU deployment, with real gains measured on AMD hardware |
| **Human–AI teaming** | None | The core of Part B. Humans set goals and guardrails, verify correctness and quality, and make the final call; AI proposes, writes code and explores the design space. The decision log is graded |

In short: MLSys·im teaches you to **predict** a system. hello-mlsys teaches you to **optimise it with AI**, and proves the result on real hardware.

## What you will learn

By the end of the programme, you will be able to:

1. **Predict.** Estimate the latency, throughput and memory use of an ML workload from first principles and with a simulator, before running it.
2. **Measure and explain.** Measure the same workload on AMD hardware with profilers, and explain in writing why the measurement differs from the prediction.
3. **Diagnose.** Identify whether a workload is limited by compute, memory bandwidth, memory capacity or communication, on devices from a laptop to a GPU cluster.
4. **Judge AI.** Evaluate an AI's diagnosis or design against the evidence, and tell when it is wrong.
5. **Lead an AI teammate.** Given a fixed target, set the goal and guardrails, direct AI through design and implementation, verify correctness and quality, and justify each decision.

## Who it is for

- Students and engineers who can train or run a model in PyTorch and want to understand how it runs on hardware.
- **Prerequisites:** Python, PyTorch basics, basic Linux, reading English documentation.
- **No GPU needed:** AMD cloud access is provided.
- **Time:** about 6–8 hours a week in Weeks 1–5, about 10 hours a week in Weeks 6–8.

## How it works

Every week follows the same loop:

```
Predict (simulator) → Build (with AI help) → Measure (AMD hardware) → Explain the gap → Decide the next experiment
```

**Part A: study and judge (Weeks 1–5, individual).** Each week has readings, one hands-on lab, and a set of *case cards*. A case card shows a real run: the prediction, the AMD measurement, the profiler trace, and an AI's diagnosis, which is sometimes wrong. You find the bottleneck, decide whether the AI is right, and predict what a change will do before the result is revealed.

**Part B: team with AI (Weeks 6–8, individual).** You work one-on-one with AI on three fixed targets. Every project is your own. For each target you:

1. **Frame** the goal, constraints and quality floor yourselves.
2. **Plan** with AI, and choose, revise or reject its proposals.
3. **Prune** infeasible designs with the simulator.
4. **Build** with AI, and review the code against the tests.
5. **Measure** on AMD hardware.
6. **Evolve:** pick the next experiment and record why.

Every decision goes into a decision log, which is graded alongside your result.

## Who does what

| Party | Role |
| --- | --- |
| **AMD** | Ground truth. Every case you study and every score you earn comes from AMD hardware. AMD also provides the ROCm stack, cloud credits, library baselines, engineer office hours and showcase judges. |
| **Simulator** | Fast predictions, and a way to rule out designs that cannot work. |
| **[Lumid](https://lum.id)** | The lab environment: case cards, measurement workflows, AI coding, suggested next experiments, and the decision log. |
| **You** | Frame the goal, judge the evidence, verify results, and decide. |

## AMD hardware tiers

| Tier | Hardware | Used to teach |
| --- | --- | --- |
| T1 | Ryzen AI PC (NPU + integrated GPU) | Heterogeneous on-device compute, energy |
| T2 | Radeon discrete GPU | Kernels, wave32/64, consumer memory limits |
| T3 | Ryzen AI Max (large CPU–GPU shared memory) | Large models on a personal device |
| T4 | Instinct MI300-class (cloud) | High-bandwidth memory, multi-GPU scale-out |

No device? AMD cloud access covers every week. Supported hardware list: **TBD**.

## Curriculum

| Week | Topic | By the end of the week, you can… | You submit | AMD |
| --- | --- | --- | --- | --- |
| **Part A** | **Study and judge** | | | |
| 1 | Systems thinking; prediction vs. measurement | Predict a model's speed on a PC and in the cloud, and explain why the measurements differ | Gap report; 3 case cards | T1, T4 |
| 2 | GPU architecture and the roofline | Say whether a workload is compute- or memory-bound on your device | Measured roofline; case cards | T1–T3 |
| 3 | Kernels and portability | Port a CUDA kernel to AMD, tune it, and explain the gap to the library | Passing kernel; case cards | T2, T4 |
| 4 | LLM inference and memory | Predict which models fit where, and how KV cache, quantisation and speculative decoding change speed | "What fits where" matrix; case cards | T3, T4 |
| 5 | Distributed serving and tail latency | Explain why a serving system misses its p99 target, and judge proposed fixes | p99 report; **Part A gate** | T4 |
| **Part B** | **Team with AI** | | | |
| 6 | Target 1: optimise a GPU kernel | Lead AI to a faster kernel without breaking correctness | Kernel scored on hidden inputs; decision log | T2, T4 |
| 7 | Target 2: meet a serving SLA on one node | Choose model size, quantisation, context, concurrency and speculative decoding to meet an SLA above a quality floor | Best configuration per tier; decision log | T4, T3 |
| 8 | Target 3: scale serving out; showcase | Extend a single-node design across GPUs and defend its cost | Result per dollar; design doc; showcase talk | T4 |

### Week-by-week content

<details>
<summary><b>Week 1: Systems thinking; prediction vs. measurement</b></summary>

**Topics**
- What makes performance a systems problem: the four limits of compute, memory bandwidth, memory capacity and communication
- Latency vs. throughput
- Back-of-envelope estimates: FLOPs and bytes moved by one forward pass
- How the simulator predicts, and where real runs deviate: launch overhead, framework cost, clocks, warm-up
- Measuring properly: warm-up runs, repeats, variance

**Readings**
- [mlsys-course, chapter 0: systems thinking](https://yuuinih.github.io/mlsys-course/modules/00-systems-thinking.html)
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: introductory chapters
- [hello-rocm](https://github.com/datawhalechina/hello-rocm): environment setup (ROCm, PyTorch, uv)

**Lab:** Install ROCm and PyTorch. Run inference for one small LLM on a personal device (T1 or T3) and on a cloud GPU (T4). Predict each run with the simulator first, then measure it.

**Case cards:** Three introductory cards on reading a prediction against a measurement.

**Submit:** A one-page gap report: your prediction, your measurement, and the three biggest reasons they differ.
</details>

<details>
<summary><b>Week 2: GPU architecture and the roofline</b></summary>

**Topics**
- AMD GPU building blocks: compute units, wavefronts (32 vs. 64 wide), registers, LDS on-chip memory
- Matrix units: MFMA on CDNA (Instinct) and WMMA on RDNA (Radeon); the XDNA NPU as a different kind of accelerator
- Memory systems: HBM on Instinct vs. LPDDR5X shared with the CPU on Ryzen AI Max
- Peak FLOPs, peak bandwidth, arithmetic intensity, and the roofline model
- Reading a profiler trace with rocprof

**Readings**
- [hello-gpu](https://github.com/datawhalechina/hello-gpu): Part 1, chapter 3, AMD GPU architecture
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: hardware acceleration
- ROCm documentation: rocprof and rocprof-compute

**Lab:** Write micro-benchmarks for memory copy and a GEMM size sweep. Plot your device's measured roofline against its spec sheet, then place GEMM, softmax and LayerNorm on it.

**Case cards:** An AI labels kernels as compute-bound or memory-bound from traces. Decide which labels are wrong and why.

**Submit:** Your roofline plot, with the three kernels placed on it and a short explanation of any gap to the spec.
</details>

<details>
<summary><b>Week 3: Kernels and portability</b></summary>

**Topics**
- The HIP programming model: grids, blocks, wavefronts
- Memory coalescing, LDS tiling, bank conflicts, occupancy
- Triton on ROCm; porting CUDA with hipify
- Code that silently assumes a 32-wide warp, and why it breaks or slows down on 64-wide wavefronts
- AMD libraries as baselines (rocBLAS, hipBLASLt, Composable Kernel); testing correctness with tolerances

**Readings**
- [hello-gpu](https://github.com/datawhalechina/hello-gpu): HIP and Triton chapters
- [hello-rocm](https://github.com/datawhalechina/hello-rocm): operator optimisation (03-infra)
- [llm-algo-leetcode](https://github.com/datawhalechina/llm-algo-leetcode): Triton exercises

**Lab:** Port a CUDA RMSNorm or softmax kernel to AMD. Get it correct, then tune its tile size and compare it with the library version.

**Case cards:** Kernels that got slower after porting. Find the cause in the trace.

**Submit:** A kernel that passes the tests, its percentage of library speed, and a short note on the remaining gap.
</details>

<details>
<summary><b>Week 4: LLM inference and memory</b></summary>

**Topics**
- Prefill vs. decode: why prefill is usually compute-bound and decode memory-bound
- KV cache size, and how to calculate it from the model shape, context length and batch
- Paged attention and continuous batching
- Quantising weights (FP8, INT4 with AWQ or GPTQ) and the KV cache; what it costs in quality
- Speculative decoding: draft models, acceptance rate, and when it helps

**Readings**
- [mlsysbook.ai](https://mlsysbook.ai) Vol I: model optimisation and serving
- [hello-rocm](https://github.com/datawhalechina/hello-rocm): deployment guides for vLLM and llama.cpp
- vLLM documentation: quantisation and speculative decoding

**Lab:** Build a "what fits where" matrix: three model sizes × three precisions on T3 and T4. Predict memory use, time to first token and time per output token, then measure them.

**Case cards:** Speculative decoding and quantisation results. Judge the AI's explanation of each.

**Submit:** The completed matrix, predicted and measured, with the biggest gaps explained.
</details>

<details>
<summary><b>Week 5: Distributed serving and tail latency</b></summary>

**Topics**
- Collective communication: all-reduce and all-gather with RCCL; link bandwidth between GPUs
- Tensor parallelism vs. pipeline parallelism vs. independent replicas
- SLOs: time to first token and time per output token, p50 vs. p99
- Queueing under load, the throughput "knee", and goodput
- Separating prefill from decode, and routing requests

**Readings**
- [mlsysbook.ai](https://mlsysbook.ai) Vol II: collective communication and inference at scale
- RCCL documentation and rccl-tests
- vLLM documentation: distributed serving

**Lab:** Sweep all-reduce message sizes with rccl-tests on a multi-GPU node. Then load-test a serving endpoint at rising arrival rates and find where p99 breaks.

**Case cards:** "More GPUs made p99 worse" and similar cases. Judge three fixes the AI proposes.

**Submit:** A p99 analysis report. Then take the Part A gate.
</details>

<details>
<summary><b>Week 6: Target 1, optimise a GPU kernel</b></summary>

**Brief:** A baseline kernel (fused attention or quantised GEMM, **TBD**) with public input shapes for development and hidden ones for scoring.

**Rules:** The core operation must not call a vendor library. Results must match the reference within tolerance.

**Your job:** Keep the AI honest on correctness while it chases speed.

**Readings:** Revisit Weeks 2–3; [hello-gpu](https://github.com/datawhalechina/hello-gpu) optimisation chapters.

**Submit:** Your best kernel, scored on the hidden shapes, and your decision log.
</details>

<details>
<summary><b>Week 7: Target 2, meet a serving SLA on one node</b></summary>

**Brief:** A fixed workload trace, an SLA of p99 time to first token < **X ms (TBD)** and p99 time per output token < **Y ms (TBD)**, and a quality floor on a fixed evaluation set (**TBD**).

**You may change:** model size, quantisation, context length, prefix caching, chunked prefill, concurrency, KV-cache memory fraction, speculative decoding.

**You may not change:** the trace, the SLA, or the evaluation set.

**Your job:** Find the best configuration on two tiers (T4 and T3) and explain why they differ. The quality floor rules out the easy win of the smallest model at the lowest precision.

**Readings:** Revisit Weeks 4–5; vLLM-ROCm documentation on serving configuration.

**Submit:** Your best configuration for each tier, its goodput at the SLA, and the decision log.
</details>

<details>
<summary><b>Week 8: Target 3, scale serving out; showcase</b></summary>

**Brief:** A heavier trace under the same SLA, with a limit on GPU count and a cost budget.

**Design choices:** replicas vs. tensor parallelism, separating prefill from decode, request routing.

**Your job:** Extend your Week 7 design and defend its cost.

**Readings:** Revisit Week 5; [mlsysbook.ai](https://mlsysbook.ai) Vol II: inference at scale.

**Submit:** Goodput at the SLA per dollar, a two-page design document, the decision log, and a short showcase talk.
</details>

## Assessment

**Part A.** Case cards are scored automatically. Lab write-ups are checked for a clear prediction, a measurement, and an explanation of the gap. The **Part A gate** in Week 5 is a set of unseen case cards, taken without AI help; you need to pass it to join Part B.

**Part B.** Each target is scored twice: by the result measured on AMD hardware, and by your decision log. A strong log shows that you:

- set the goal, constraints and quality floor yourselves, before asking the AI for plans;
- accepted or overrode AI proposals based on evidence, and caught it gaming the metric;
- checked correctness and quality before claiming any speedup;
- tracked how accurate your predictions were, and explained the misses.

**Using AI.** AI help is encouraged everywhere except the Part A gate. You must be able to explain and verify everything you submit.

### To complete the programme

- Case cards for at least 4 of the 5 Part A weeks, and a pass on the Part A gate.
- At least 2 of the 3 Part B targets, each with a decision log.
- A review of one other participant's decision log.

**You leave with** a portfolio of gap reports and decision logs, a place on a public leaderboard measured on AMD hardware, and a completion certificate. Top participants present at the showcase and are eligible for AMD prizes.

## Getting started

1. Set up ROCm and PyTorch by following the [hello-rocm environment guide](https://github.com/datawhalechina/hello-rocm), or use AMD cloud access if you have no supported device.
2. Create a Lumid account (**link TBD**) and open the Week 1 lab.
3. Join the cohort group (**TBD**).

## Project structure (planned)

```
hello-mlsys/
├── README.md
├── docs/
│   ├── week01-systems-thinking/
│   ├── week02-architecture-roofline/
│   ├── week03-kernels-portability/
│   ├── week04-llm-inference-memory/
│   ├── week05-distributed-serving/
│   ├── week06-target-kernel/
│   ├── week07-target-sla-serving/
│   └── week08-target-scale-out/
├── labs/            # lab starter code and tests
├── case-cards/      # case card index (cards are served through Lumid)
├── targets/         # Part B briefs, baselines, traces, evaluation sets
└── assets/
```

## Roadmap

- [x] Curriculum design
- [ ] Hardware whitelist and cloud credits confirmed with AMD
- [ ] Week 1–5 labs and case cards (about 10 cards per week)
- [ ] Part B targets: baselines, hidden tests, traces, SLA thresholds, quality floor
- [ ] Lumid lab apps and leaderboard
- [ ] Pilot cohort

## Contributing

Issues and pull requests are welcome, especially new case cards drawn from real AMD runs, lab improvements, and test results on hardware not yet covered.

## Acknowledgements

hello-mlsys builds on [CS249r / MLSys·im](https://github.com/harvard-edge/cs249r_book), [hello-gpu](https://github.com/datawhalechina/hello-gpu), [hello-rocm](https://github.com/datawhalechina/hello-rocm), [llm-algo-leetcode](https://github.com/datawhalechina/llm-algo-leetcode), [mlsys-course](https://github.com/YuuinIH/mlsys-course), and the [Machine Learning Systems course at NUS](https://mlsys.io/MLsys_25Sem2.html). Hardware and cloud access are provided by AMD.

## License

**TBD**
