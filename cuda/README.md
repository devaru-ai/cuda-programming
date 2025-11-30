# **Phase 1: The Hardware & Syntax Foundation**
*Goal: Prove you understand how the software maps to the hardware.*

## **Topic 1.1: Thread Indexing & Mapping**
* **Concepts:** Grid/Block/Thread hierarchy, 1D/2D/3D indexing, mapping threads to data.
* **Program 1: 3D Indexing Utility**
    * **Task:** Write a kernel that prints the global ID of a thread in a 3D grid of 3D blocks.
    * **Success Metric:** Verify mathematically that your calculated ID is unique for every thread.

## **Topic 1.2: Global Memory & Coalescing**
* **Concepts:** DRAM bursts, cache lines (128 bytes), uncoalesced access penalties.
* **Program 2: Matrix Transpose (Coalescing Benchmark)**
    * **Task:** Implement two versions of Matrix Transpose.
        1.  *Naive:* Read Row $\to$ Write Column (writes are uncoalesced).
        2.  *Coalesced:* Read Row $\to$ Shared Mem Tile $\to$ Write Row (transposed indices).
    * **Success Metric:** Version 2 must be at least **5x faster** than Version 1 on large matrices ($N > 4096$).

## **Topic 1.3: Asynchronous Execution**
* **Concepts:** Host-Device synchronization, CUDA Streams, overlap.
* **Program 3: Vector Add with Streams**
    * **Task:** Process a 1GB array in 4 chunks using 4 CUDA streams.
    * **Constraint:** Use `cudaMemcpyAsync`.
    * **Success Metric:** In Nsight Systems, you must see "Copy H2D" and "Kernel Compute" overlapping on the timeline.



# **Phase 2: The Optimization "Bar Raiser"**
*Goal: Pass the technical deep-dive. This is where you demonstrate engineering intuition.*

## **Topic 2.1: Shared Memory & Banking**
* **Concepts:** Shared memory banks (32 banks, 4 bytes wide), Bank Conflicts, Padding.
* **Program 4: Tiled Matrix Multiplication (SGEMM)**
    * **Task:** Implement $C = A \times B$ using Shared Memory tiling.
    * **Constraint:** Tile size must be typically 16x16 or 32x32.
    * **Success Metric:** Achieve >50% of the theoretical peak performance of a naive CuBLAS call (or significant speedup over global memory version).
    * **Bonus:** Add code to explicitly handle/avoid bank conflicts if your tile size causes them.

## **Topic 2.2: Warp Primitives & Divergence**
* **Concepts:** SIMT execution, Warp Divergence, Warp Shuffles (`__shfl_down_sync`).
* **Program 5: Warp-Level Reduction (Sum)**
    * **Task:** Sum 1024 integers using *only* warp shuffles for the last 32 elements.
    * **Constraint:** Do not use Shared Memory for the final warp reduction.
    * **Success Metric:** Code compiles without `__syncthreads()` inside the warp loop.

## **Topic 2.3: Atomic Operations**
* **Concepts:** Read-Modify-Write, serialization in L2 cache.
* **Program 6: Privatized Histogram**
    * **Task:** Compute a histogram of pixel values (0-255) for a large image.
    * **Constraint:** Create a local histogram in Shared Memory first, *then* atomically add to the global histogram.
    * **Success Metric:** Performance should not degrade linearly with the number of threads (avoids global atomic contention).


# Phase 3: The AI Specialist 
*Goal: Ace the specific role you are applying for (High-Performance AI).*

## **Topic 3.1: Softmax (The Transformer Bottleneck)**
* **Concepts:** Reduction + Map, Online Softmax (Safe Softmax).
* **Program 7: Safe Softmax Kernel**
    * **Task:** Implement Softmax: $\frac{e^{x_i - max(x)}}{\sum e^{x_j - max(x)}}$.
    * **Constraint:** Must handle numerical stability (subtracting max).
    * **Expert Level:** Implement "Online Softmax" (compute max and sum in a single pass).

## **Topic 3.2: LayerNorm / RMSNorm**
* **Concepts:** Row-wise reduction, variance calculation, memory bandwidth bound.
* **Program 8: Fused LayerNorm**
    * **Task:** Compute Mean, Variance, and Normalize $x$ in one kernel.
    * **Success Metric:** Compare "Global Memory Load Efficiency" in Nsight Compute against a naive implementation (two passes).

## **Topic 3.3: Sparse Operations (Your Research Interest)**
* **Concepts:** CSR/COO formats, Load Balancing.
* **Program 9: SpMV (Sparse Matrix-Vector Multiplication)**
    * **Task:** Implement $y = A \times x$ where A is in CSR format.
    * **Challenge:** Handle the case where one row has 1 element and the next has 10,000 (thread divergence/load imbalance).
    * **Solution:** Implement "CSR-Vector" or "CSR-Stream" logic.

## **Topic 3.4: Tensor Cores (Hardware Acceleration)**
* **Concepts:** Mixed Precision (FP16/BF16), WMMA API (`nvcuda::wmma`), mma.sync.
* **Program 10: FP16 Matrix Mul with WMMA**
    * **Task:** Use the `wmma` intrinsics to perform a 16x16x16 matrix multiplication.
    * **Success Metric:** Successfully compile and run using `half` precision types.


# Phase 4: Theoretical & System Design Knowledge
*You don't need to code these, but you must know the answers immediately.*

1.  **Architecture Specs:**
    * **H100 vs A100:** Know the key difference (H100 has TMA - Tensor Memory Accelerator, Transformer Engine, and Distributed Shared Memory).
    * **L1 vs L2 Cache:** L1 is per SM (configurable as Shared Mem), L2 is shared across all SMs.
2.  **Profiling Metrics (Nsight Compute):**
    * **Memory Bound:** High DRAM throughput, low Compute throughput.
    * **Compute Bound:** High SM throughput, low DRAM throughput.
    * **Occupancy:** Ratio of active warps to maximum warps per SM.
3.  **Triton vs. CUDA:**
    * *Answer:* "Triton compiles block-level logic to optimized LLVM IR, handling memory coalescing and shared memory management automatically. CUDA gives thread-level control but requires manual management of those resources."
