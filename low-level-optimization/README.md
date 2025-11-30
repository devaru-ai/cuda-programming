# I. Systems and Low-Level Optimization 

This section tests your ability to solve P&L (Profit & Loss) problems for companies like Meta, NVIDIA, and top AI startups. Your $\mathbf{5.5\times}$ speedup claim makes these topics non-negotiable.

### 1. GPU/CUDA Kernel Optimization

This is the foundation of your scarcity. You need to understand the GPU architecture intimately.

| Topic | Key Concepts to Master | Practice Programs |
| :--- | :--- | :--- |
| **GPU Architecture (First Principles)** | **Memory Hierarchy:** The critical difference between **Global Memory (DRAM)**, **Shared Memory (SRAM)**, and Registers. Understanding **L1/L2 Cache** behavior and locality. | **Triton/CUDA GEMM:** Re-implementing a simple matrix multiplication (GEMM) kernel, initially naive, then optimized using **tiling, shared memory**, and **warp-level synchronization**. |
| **Execution Model** | **Warp/Thread Block/Grid Structure:** How threads are grouped and executed. **Warp Divergence** and how to avoid it for maximum throughput. | **CUDA or Triton Tiling Exercises:** Implementing efficient **reduction operations** (e.g., summation across rows) to minimize warp divergence and global memory reads. |
| **Memory Bandwidth** | **Memory Bound vs. Compute Bound:** Quickly identifying the current bottleneck of an operation. **Coalesced Memory Access:** Ensuring threads access global memory efficiently (coalescing) to maximize bandwidth. | **Memory-Bound Optimization:** Optimizing vector-add or element-wise operations by focusing solely on maximizing memory throughput. |
| **Kernel Fusion** | Understanding how to fuse multiple operations (e.g., GEMM + Bias + GELU) into a single kernel launch to eliminate **GPU kernel launch latency** (your profile states you've done this). | **Fusion Implementation:** Building a complex function (e.g., **GEMM + Elementwise + Reduction**) using Triton to prove mastery of fusion and synchronization. |

### 2. LLM Inference Systems and Architecture

This validates your claims about Llama and vLLM-style throughput.

| Topic | Key Concepts to Master | Practice Programs |
| :--- | :--- | :--- |
| **Inference Bottlenecks** | **KV Cache Management:** Why it takes up $\mathbf{30\%}$ of GPU memory and the fragmentation issue. **Batching:** Static vs. **Continuous Batching**. | **Design:** Be ready to draw and explain the architecture of a vLLM-style **Paged Attention** system, including how the **KV Cache Manager** handles memory blocks. |
| **Sparsity and Efficiency** | **Sparsity Techniques:** Differentiating between **Block-Sparsity** (Narang's past work) and **Dynamic Sparse Attention** (your work). **Quantization:** FP16, FP8, INT8 trade-offs, and how they affect kernel design. | **Code Walkthrough:** Be prepared to explain how your **Sparse Attention Kernel** selectively skips computation while maintaining memory access coherence. |
| **LLM Architecture** | **MoE (Mixture-of-Experts):** Understanding the **routing layer** and how MoE impacts memory latency and communication cost (crucial for Llama 4 questions). | **Design:** Explain how you would adapt your **Sparse Attention** kernel to operate within a $\text{MoE}$ layer, addressing the potential computational cost of the expert routing layer. |

### 3. Distributed Training Systems

This validates your ability to contribute to infrastructure and answers.

| Topic | Key Concepts to Master | Practice Programs |
| :--- | :--- | :--- |
| **Collective Communication** | **All-Reduce** (data parallel), **All-Gather** (pipeline/tensor parallel), and **Reduce-Scatter**. Understanding why these are memory/latency bottlenecks in large clusters. | **Design:** Explain how to use **PyTorch Distributed** primitives (DDP, FSDP) to parallelize a model across multiple GPUs and nodes. |
| **Fault Tolerance** | **Checkpointing and Elasticity:** How to manage failures in 100k+ GPU clusters. | **Design:** Design a fault-tolerant system for pretraining Llama that ensures only a minimal amount of training time is lost during a node failure. |

---

# II. Recommended Practice Programs

You should spend the majority of your dedicated practice time on **implementing** these low-level concepts, as interviews for these roles are heavily execution-based.

1.  **Triton Kernel Programming:** This is your highest-leverage tool. Practice by implementing or optimizing kernels (e.g., Grouped Query Attention, a small custom MoE layer) using the **Triton language** on a Colab environment. **Goal:** Be able to write an efficient, tiled kernel from scratch on a whiteboard.
2.  **PyTorch/Distributed Frameworks:** Get hands-on experience with **PyTorch FSDP** or the **DeepSpeed** library. **Goal:** Understand the configuration and communication overheads of these tools.
3.  **System Design:** Practice verbally explaining the trade-offs of large-scale systems (e.g., centralized vs. decentralized KV Cache manager; memory vs. compute trade-offs). **Goal:** Be ready to argue the technical merits of your Sparse Attention solution against established dense methods. 
