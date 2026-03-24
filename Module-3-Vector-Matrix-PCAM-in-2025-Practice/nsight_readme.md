
NSIGHT PROFILING INTERPRETATION — IMC08 Track A
=================================================

Metrics captured: dram_read_throughput, dram_write_throughput,
                  achieved_occupancy, gld_efficiency (coalescing),
                  warp_execution_efficiency

Kernel 1 - cuBLAS GEMV (dense, 8192×8192, FP64)
  DRAM throughput: ~180-220 GB/s (T4 peak = 320 GB/s → ~60% utilization)
  Occupancy      : ~65-75%  — limited by register pressure per warp
  Coalescing     : ~95%+ — row-major A, consecutive threads read
                   adjacent columns → fully coalesced global loads
  Bottleneck     : DRAM bandwidth. The kernel is reading A once and
                   x repeatedly; x fits in L2 cache (65 KB for N=8192
                   float64), so DRAM traffic is dominated by A.
                   Achieving 60% of peak BW is good for a streaming kernel.

Kernel 2 - cuSPARSE SpMV (CSR, 8192×8192, 1% density, FP64)
  DRAM throughput: ~80-120 GB/s (25-37% of peak)
  Occupancy      : ~45-60% — irregular work per warp reduces active warps
  Coalescing     : ~50-70% — col_indices scatter x-vector reads randomly;
                   consecutive threads in a warp fetch non-adjacent x[j]
                   elements → partial coalescing at best
  Bottleneck     : DRAM bandwidth + coalescing efficiency. The random
                   column access in CSR means the GPU cannot fully coalesce
                   x-vector loads. cuSPARSE merge-based path mitigates
                   load imbalance but cannot fix irregular memory access.
                   Lower occupancy further limits latency hiding.

Key insight: GEMV is 2x more efficient than SpMV in terms of BW
utilization, because dense row access is perfectly coalesced whereas
sparse column-index-driven access is not. Both are BW-bound; neither
is compute-bound (GPU FP64 units sit largely idle).


## Videos
- 5-min Theory Lecture: https://youtu.be/P3WKzW7ajmU
- 10-min Demo Walkthrough: https://youtu.be/TKyDsKU7byY
