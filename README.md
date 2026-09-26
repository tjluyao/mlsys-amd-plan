# hello-mlsys

中文 | [English](./README_en.md)

**在 AMD 硬件上学习机器学习系统：先预测性能，再真机实测，最后与 AI 协作把系统做得更好。**

hello-mlsys 是一个为期 8 周的社区学习计划。每周你都要先预测一个 ML 工作负载的性能，再在真实的 AMD 硬件上测量它（从 Ryzen AI 笔记本到 Instinct 云端 GPU），并解释预测与实测之间的差距。后半程，你将与 AI 组队完成固定的工程目标，所有成绩都在 AMD 硬件上评测。

> 状态：课程设计阶段。标注 **待定** 的内容将在首期开营前确认。

## 为什么是 hello-mlsys

| 项目 | 教什么 | 与 hello-mlsys 的关系 |
| --- | --- | --- |
| [hello-rocm](https://github.com/datawhalechina/hello-rocm) | 用好 AMD GPU：环境配置、模型部署、微调、HIP 算子 | 作为环境配置与部署部分的阅读材料 |
| [hello-gpu](https://github.com/datawhalechina/hello-gpu) | AMD 上的 GPU 算子编程 | 作为体系结构与算子部分的阅读材料 |
| [MLSys·im](https://github.com/harvard-edge/cs249r_book)（CS249r） | 用模拟器预测系统性能 | 用它做预测，再在真机上验证 |
| **hello-mlsys** | **从整体上分析 ML 系统，并引导 AI 改进系统** | 把三者串起来：预测、实测、解释、决策 |

### 与 MLSys·im 的区别

MLSys·im 是一个优秀的性能预测工具，hello-mlsys 把它作为起点，并在四个方面更进一步：

| | MLSys·im | hello-mlsys |
| --- | --- | --- |
| **交互性** | 调参数、看模拟结果 | 每一步都有反馈：预测 → AMD 真机实测 → 解释差距 → 选择下一个实验；案例卡让你在结果揭晓前先下判断 |
| **AI 能力** | 无 | 在 Lumid 中内置 AI 编程与方案规划；AI 给出诊断、设计和下一步实验建议，由你评判取舍 |
| **优化空间** | 分析给定配置的瓶颈 | 真正动手优化：算子代码、量化、上下文长度、并发、投机解码、并行方式、多卡部署，并在 AMD 硬件上看到真实收益 |
| **人机协作** | 无 | Part B 以人机组队为核心：人设定目标与约束、验证正确性与质量、做最终决策；AI 负责提方案、写代码、探索设计空间；决策日志纳入考核 |

简而言之：MLSys·im 教你**预测**系统，hello-mlsys 教你**与 AI 一起优化**系统，并在真实硬件上验证。

## 学习目标

完成本计划后，你将能够：

1. **预测**：在运行之前，从第一性原理出发并借助模拟器，估算 ML 工作负载的延迟、吞吐和显存占用。
2. **实测与解释**：用性能分析工具在 AMD 硬件上测量同一工作负载，并书面解释实测与预测为何不同。
3. **诊断**：在从笔记本到 GPU 集群的各类设备上，判断工作负载的瓶颈是算力、内存带宽、内存容量还是通信。
4. **评判 AI**：依据证据评估 AI 给出的诊断或设计，并识别它何时出错。
5. **引导 AI 队友**：面对固定目标，设定目标与约束，引导 AI 完成设计与实现，验证正确性与质量，并说明每一个决策的理由。

## 适合谁

- 会用 PyTorch 训练或运行模型、希望理解模型在硬件上如何运行的学生和工程师。
- **前置要求**：Python、PyTorch 基础、Linux 基础、能阅读英文文档。
- **无需自备 GPU**：提供 AMD 云端算力。
- **时间投入**：第 1–5 周每周约 6–8 小时，第 6–8 周每周约 10 小时。

## 学习方式

每周都遵循同一个循环：

```
预测（模拟器）→ 构建（AI 辅助）→ 实测（AMD 硬件）→ 解释差距 → 决定下一个实验
```

**Part A：学习与评判（第 1–5 周，个人完成）**。每周包括阅读材料、一个动手实验和一组 **案例卡**。每张案例卡展示一次真实运行：预测值、AMD 实测值、性能分析 trace，以及一份 AI 给出的诊断（有时是错的）。你需要找出瓶颈，判断 AI 是否正确，并在结果揭晓前预测某项改动的效果。

**Part B：与 AI 组队（第 6–8 周，个人完成）**。你与 AI 一对一协作，完成三个固定目标，所有项目均为个人项目。每个目标都按以下步骤进行：

1. **界定**：由你自己设定目标、约束和质量底线。
2. **规划**：与 AI 一起制定方案，选择、修改或否决它的提议。
3. **剪枝**：用模拟器排除不可行的设计。
4. **构建**：由 AI 编写代码，你对照测试进行审查。
5. **实测**：在 AMD 硬件上测量。
6. **迭代**：选择下一个实验，并记录理由。

每个决策都会记入 **决策日志**，与最终成绩一起评分。

## 各方分工

| 角色 | 职责 |
| --- | --- |
| **AMD** | 提供真实基准。你分析的每个案例、获得的每个成绩都来自 AMD 硬件。AMD 同时提供 ROCm 软件栈、云端算力额度、算子库基线、工程师答疑和结营评审。 |
| **模拟器** | 快速给出预测，并排除不可行的设计。 |
| **[Lumid](https://lum.id)** | 实验环境：案例卡、测量工作流、AI 编程、下一步实验建议和决策日志。 |
| **你** | 界定目标、评判证据、验证结果、做出决策。 |

## AMD 硬件档位

| 档位 | 硬件 | 用于教学 |
| --- | --- | --- |
| T1 | Ryzen AI PC（NPU + 集成显卡） | 端侧异构计算、能耗 |
| T2 | Radeon 独立显卡 | 算子开发、wave32/64、消费级显存限制 |
| T3 | Ryzen AI Max（CPU 与 GPU 共享大内存） | 在个人设备上运行大模型 |
| T4 | Instinct MI300 级（云端） | 高带宽显存、多卡扩展 |

没有设备？AMD 云端算力可覆盖全部 8 周。支持的硬件清单：**待定**。

## 课程安排

| 周次 | 主题 | 本周结束时，你能够…… | 提交内容 | AMD |
| --- | --- | --- | --- | --- |
| **Part A** | **学习与评判** | | | |
| 1 | 系统思维：预测与实测 | 预测同一模型在 PC 和云端的速度，并解释实测为何不同 | 差距报告；3 张案例卡 | T1, T4 |
| 2 | GPU 体系结构与 Roofline | 判断一个工作负载在你的设备上是算力受限还是访存受限 | 实测 Roofline；案例卡 | T1–T3 |
| 3 | 算子开发与可移植性 | 把一个 CUDA 算子移植到 AMD、完成调优，并解释与算子库的差距 | 通过测试的算子；案例卡 | T2, T4 |
| 4 | LLM 推理与显存 | 预测哪些模型能在哪些设备上运行，以及 KV Cache、量化和投机解码如何影响速度 | "什么能装进哪里"矩阵；案例卡 | T3, T4 |
| 5 | 分布式推理服务与尾延迟 | 解释服务系统为何达不到 p99 目标，并评判改进方案 | p99 分析报告；**Part A 关卡** | T4 |
| **Part B** | **与 AI 组队** | | | |
| 6 | 目标 1：优化 GPU 算子 | 引导 AI 写出更快的算子，同时保证正确性 | 在隐藏输入上评测的算子；决策日志 | T2, T4 |
| 7 | 目标 2：单机满足推理服务 SLA | 选择模型规模、量化、上下文长度、并发和投机解码，在质量底线之上满足 SLA | 各档位的最佳配置；决策日志 | T4, T3 |
| 8 | 目标 3：多卡扩展与结营展示 | 把单机方案扩展到多卡，并论证其成本 | 单位成本下的结果；设计文档；结营展示 | T4 |

### 每周内容

<details>
<summary><b>第 1 周：系统思维：预测与实测</b></summary>

**主题**
- 为什么性能是系统问题：算力、内存带宽、内存容量、通信四类限制
- 延迟与吞吐
- 粗略估算：一次前向计算的 FLOPs 与数据搬运量
- 模拟器如何预测，以及真实运行为何偏离：kernel 启动开销、框架开销、频率、预热
- 正确的测量方法：预热、重复测量、方差

**阅读**
- [mlsys-course 第 0 章：系统思维](https://yuuinih.github.io/mlsys-course/modules/00-systems-thinking.html)
- [mlsysbook.ai](https://mlsysbook.ai) 第一卷：导论章节
- [hello-rocm](https://github.com/datawhalechina/hello-rocm)：基础环境配置（ROCm、PyTorch、uv）

**实验**：安装 ROCm 和 PyTorch。分别在个人设备（T1 或 T3）和云端 GPU（T4）上运行一个小型 LLM 的推理。每次运行前先用模拟器预测，再实测。

**案例卡**：3 张入门卡，练习对照预测解读实测结果。

**提交**：一页差距报告，包括你的预测、实测结果，以及造成差异的三个最主要原因。
</details>

<details>
<summary><b>第 2 周：GPU 体系结构与 Roofline</b></summary>

**主题**
- AMD GPU 的基本组成：计算单元（CU）、wavefront（32 宽与 64 宽）、寄存器、LDS 片上存储
- 矩阵单元：CDNA（Instinct）上的 MFMA 与 RDNA（Radeon）上的 WMMA；XDNA NPU 作为另一类加速器
- 存储系统：Instinct 上的 HBM 与 Ryzen AI Max 上与 CPU 共享的 LPDDR5X
- 峰值算力、峰值带宽、算术强度与 Roofline 模型
- 用 rocprof 解读性能分析 trace

**阅读**
- [hello-gpu](https://github.com/datawhalechina/hello-gpu)：第一部分第 3 章，AMD GPU 体系结构
- [mlsysbook.ai](https://mlsysbook.ai) 第一卷：硬件加速
- ROCm 文档：rocprof 与 rocprof-compute

**实验**：编写内存拷贝和 GEMM 尺寸扫描的微基准测试。绘制设备的实测 Roofline 并与规格对比，再把 GEMM、softmax 和 LayerNorm 标注在图上。

**案例卡**：AI 根据 trace 判断算子是算力受限还是访存受限。找出哪些判断是错的，并说明原因。

**提交**：Roofline 图，标注三个算子，并简要解释与规格值的差距。
</details>

<details>
<summary><b>第 3 周：算子开发与可移植性</b></summary>

**主题**
- HIP 编程模型：grid、block、wavefront
- 访存合并、LDS 分块、bank 冲突、占用率
- ROCm 上的 Triton；用 hipify 移植 CUDA 代码
- 默认假设 warp 为 32 宽的代码，在 64 宽 wavefront 上为何会出错或变慢
- 以 AMD 算子库（rocBLAS、hipBLASLt、Composable Kernel）为基线；带容差的正确性测试

**阅读**
- [hello-gpu](https://github.com/datawhalechina/hello-gpu)：HIP 与 Triton 章节
- [hello-rocm](https://github.com/datawhalechina/hello-rocm)：算子优化（03-infra）
- [llm-algo-leetcode](https://github.com/datawhalechina/llm-algo-leetcode)：Triton 练习

**实验**：把一个 CUDA 版的 RMSNorm 或 softmax 算子移植到 AMD。先保证正确，再调优分块大小，并与算子库版本对比。

**案例卡**：移植后变慢的算子。从 trace 中找出原因。

**提交**：通过测试的算子、它达到算子库速度的百分比，以及对剩余差距的简要说明。
</details>

<details>
<summary><b>第 4 周：LLM 推理与显存</b></summary>

**主题**
- Prefill 与 Decode：为什么 prefill 通常是算力受限，而 decode 是访存受限
- KV Cache 大小，以及如何根据模型结构、上下文长度和 batch 计算它
- Paged Attention 与连续批处理
- 权重量化（FP8、基于 AWQ 或 GPTQ 的 INT4）与 KV Cache 量化，以及它们对质量的影响
- 投机解码：草稿模型、接受率，以及何时有效

**阅读**
- [mlsysbook.ai](https://mlsysbook.ai) 第一卷：模型优化与推理服务
- [hello-rocm](https://github.com/datawhalechina/hello-rocm)：vLLM 与 llama.cpp 部署指南
- vLLM 文档：量化与投机解码

**实验**：构建"什么能装进哪里"矩阵：3 种模型规模 × 3 种精度，分别在 T3 和 T4 上运行。先预测显存占用、首 token 延迟和单 token 输出时间，再实测。

**案例卡**：投机解码与量化的实验结果。评判 AI 对每个结果的解释。

**提交**：完整的矩阵，包含预测值与实测值，并解释最大的几处差距。
</details>

<details>
<summary><b>第 5 周：分布式推理服务与尾延迟</b></summary>

**主题**
- 集合通信：用 RCCL 实现 all-reduce 与 all-gather；GPU 之间的链路带宽
- 张量并行、流水线并行与多副本的取舍
- SLO：首 token 延迟与单 token 输出时间，p50 与 p99
- 负载下的排队、吞吐"拐点"与 goodput（满足 SLO 的有效吞吐）
- Prefill 与 Decode 分离，以及请求路由

**阅读**
- [mlsysbook.ai](https://mlsysbook.ai) 第二卷：集合通信与大规模推理
- RCCL 文档与 rccl-tests
- vLLM 文档：分布式推理服务

**实验**：在多卡节点上用 rccl-tests 扫描不同消息大小的 all-reduce 性能。然后逐步提高请求到达率，对推理服务做压力测试，找到 p99 失守的位置。

**案例卡**："加了 GPU 反而 p99 更差"等案例。评判 AI 提出的三个修复方案。

**提交**：p99 分析报告。然后完成 Part A 关卡。
</details>

<details>
<summary><b>第 6 周：目标 1，优化 GPU 算子</b></summary>

**任务**：一个基线算子（融合注意力或量化 GEMM，**待定**），提供公开输入尺寸用于开发，另有隐藏尺寸用于评分。

**规则**：核心计算不得调用厂商算子库；结果须在容差范围内与参考实现一致。

**你的职责**：在 AI 追求速度的同时，守住正确性。

**阅读**：回顾第 2–3 周；[hello-gpu](https://github.com/datawhalechina/hello-gpu) 优化章节。

**提交**：你的最佳算子（在隐藏尺寸上评测）以及决策日志。
</details>

<details>
<summary><b>第 7 周：目标 2，单机满足推理服务 SLA</b></summary>

**任务**：一份固定的负载 trace；SLA 为 p99 首 token 延迟 < **X ms（待定）**、p99 单 token 输出时间 < **Y ms（待定）**；以及在固定评测集上的质量底线（**待定**）。

**可以调整**：模型规模、量化、上下文长度、前缀缓存、分块 prefill、并发、KV Cache 显存占比、投机解码。

**不可调整**：负载 trace、SLA 和评测集。

**你的职责**：分别在 T4 和 T3 上找到最佳配置，并解释两者为何不同。质量底线排除了"选最小模型加最低精度"这种取巧做法。

**阅读**：回顾第 4–5 周；vLLM-ROCm 服务配置文档。

**提交**：每个档位的最佳配置、它在 SLA 下的 goodput，以及决策日志。
</details>

<details>
<summary><b>第 8 周：目标 3，多卡扩展与结营展示</b></summary>

**任务**：在相同 SLA 下承载更重的负载 trace，并限制 GPU 数量和成本预算。

**设计选择**：多副本还是张量并行、Prefill 与 Decode 分离、请求路由。

**你的职责**：在第 7 周方案的基础上扩展，并论证其成本。

**阅读**：回顾第 5 周；[mlsysbook.ai](https://mlsysbook.ai) 第二卷：大规模推理。

**提交**：单位成本下满足 SLA 的 goodput、两页设计文档、决策日志，以及一场简短的结营展示。
</details>

## 考核方式

**Part A**：案例卡自动评分。实验报告需要包含明确的预测、实测结果以及对差距的解释。第 5 周的 **Part A 关卡** 是一组未见过的案例卡，须独立完成、不得使用 AI；通过后才能进入 Part B。

**Part B**：每个目标从两方面评分：在 AMD 硬件上测得的结果，以及你的决策日志。一份好的决策日志应体现你：

- 在向 AI 要方案之前，自己设定了目标、约束和质量底线；
- 基于证据采纳或否决 AI 的提议，并发现了它钻指标空子的行为；
- 在宣称任何加速之前，先验证了正确性和质量；
- 记录了预测的准确程度，并解释了预测失误的原因。

**关于使用 AI**：除 Part A 关卡外，全程鼓励使用 AI。但你必须能解释并验证自己提交的一切内容。

### 结营要求

- 至少完成 Part A 中 4 周的案例卡，并通过 Part A 关卡。
- 至少完成 Part B 三个目标中的 2 个，且每个都附有决策日志。
- 评审另一位学员的决策日志。

**你将收获**：一套由差距报告和决策日志组成的作品集、在 AMD 硬件实测排行榜上的名次，以及结营证书。表现最好的学员将在结营展示中演讲，并有机会获得 AMD 奖品。

## 快速开始

1. 按照 [hello-rocm 环境配置指南](https://github.com/datawhalechina/hello-rocm) 配置 ROCm 和 PyTorch；没有支持的设备可使用 AMD 云端算力。
2. 注册 Lumid 账号（**链接待定**），打开第 1 周实验。
3. 加入学习群（**待定**）。

## 项目结构（规划中）

```
hello-mlsys/
├── README.md
├── README_en.md
├── docs/
│   ├── week01-systems-thinking/       # 系统思维
│   ├── week02-architecture-roofline/  # 体系结构与 Roofline
│   ├── week03-kernels-portability/    # 算子与可移植性
│   ├── week04-llm-inference-memory/   # LLM 推理与显存
│   ├── week05-distributed-serving/    # 分布式推理服务
│   ├── week06-target-kernel/          # 目标 1：算子优化
│   ├── week07-target-sla-serving/     # 目标 2：SLA 推理服务
│   └── week08-target-scale-out/       # 目标 3：多卡扩展
├── labs/            # 实验起始代码与测试
├── case-cards/      # 案例卡索引（案例卡通过 Lumid 下发）
├── targets/         # Part B 任务说明、基线、trace、评测集
└── assets/
```

## 路线图

- [x] 课程设计
- [ ] 与 AMD 确认硬件清单与云端算力额度
- [ ] 第 1–5 周实验与案例卡（每周约 10 张）
- [ ] Part B 目标：基线、隐藏测试、trace、SLA 阈值、质量底线
- [ ] Lumid 实验应用与排行榜
- [ ] 试点学期

## 参与贡献

欢迎提交 Issue 和 PR，尤其是来自 AMD 真机运行的新案例卡、实验改进，以及在尚未覆盖的硬件上的测试结果。

## 致谢

hello-mlsys 建立在以下项目之上：[CS249r / MLSys·im](https://github.com/harvard-edge/cs249r_book)、[hello-gpu](https://github.com/datawhalechina/hello-gpu)、[hello-rocm](https://github.com/datawhalechina/hello-rocm)、[llm-algo-leetcode](https://github.com/datawhalechina/llm-algo-leetcode)、[mlsys-course](https://github.com/YuuinIH/mlsys-course)，以及 [新加坡国立大学机器学习系统课程](https://mlsys.io/MLsys_25Sem2.html)。硬件与云端算力由 AMD 提供。

## 许可证

**待定**
