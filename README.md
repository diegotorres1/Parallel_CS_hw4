# Effect of Stride on memory hierarchy

### 3.3. Measuring Parameters of the TLB
The phenomena we observe when we consider the TLB are the same as for the cache; the only
difference is in the particular values of N and s where the changes in behavior occur. The measurements
we present in the next section show the behavior of both the cache and the TLB when both are active, and
in some regions their effects overlap. In all cases, however, it is relatively straightforward to isolate the
effects of one from the other.
### 4. Experimental Results for Caches and TLBs
We ran our cache evaluation benchmark on several computers, and we show the results in figures
2-4 and in table 2. The graphs shown in the figures show the average time per iteration as a function of
the size of the array and the stride, while table 2 summarizes the cache and TLB parameters extracted
from the profiles. The units used in the graphs are: bytes for measures of size, and nanoseconds for time
related magnitudes. Each curve on each of the graphs correspond to a particular array size (N), while the
horizontal axis represents different stride values (s). We only shown curves for array sizes that are
greater or equal to the size of the cache.
The four basic regimes for the different cache and TLB miss patterns can be seen clearly in most of
the figures. A very clear example is the results for the IBM RS/6000 530 (fig. 2, lower-right graph). On
this machine, regime 1 is represented by the curve labeled 64K. The other three regimes of cache misses
are in the three curves with labels 128K, 256K, and 512K. The first segment on each curve, where the
value of the average time per iteration increases in proportion to s, corresponds to regime 2.a. The next
segment of the curve, where the time per iteration is almost constant, corresponds to regime 2.b, and the
sudden drop in the time per iteration at the end is where regime 2.c starts. In the same graph, curves for
array sizes of 1M, 2M, and 4M show the same regimes for both the cache and TLB.
The results in table 2 for the DEC 3100, DEC 5400, MIPS M/2000, and DEC 5500 show the differences in their cache organizations. These four machines use the R2000/R2001 or R3000/R3001 processors from MIPS Corporation (now part of Silicon Graphics Inc.). All have a 64KB direct mapped cache
and a fully-associative TLB with 64 entries with an entry granularity of 4096 bytes. The main difference
9
between their respective caches are the line size and the miss penalty. The DEC 3100 has the smallest
line size having only 4 bytes [Furl90]; the DEC 5400 and 5500 have line sizes of 16 bytes, and the MIPS
M/2000 has the largest line size of 64 bytes. The miss penalty per line also shows a wide range of values,
from 540 ns for the DEC 3100 to 1680 ns for the DEC 5400.
It is interesting to compare the ratios between the cache and TLB penalty misses and the execution
time of a single iteration with no misses, which are given in table 2. Although the no-miss time of the
test is not a good measure of the true speed of the processor, it at least gives an indication of the basic
floating-point performance (add and multiply) and helps to put in perspective the miss penalties. The
results show a large variation in the ratio of the cache penalty to the no-miss iteration time, ranging from
0.51 on the Sparcstation 1+ to 4.00 on the VAX 9000. In the VAX 9000, loading a cache line takes four
times longer than the no-miss iteration time. With respect to TLB misses the range of values goes from
0.53 to 6.35, with the highest value corresponding to the IBM RS/6000 530.
Note that a high miss penalty does not necessarily reflect a bad cache design. The miss penalty may
be high as a tradeoff for a low miss ratio, as the result of cost/performance tradeoffs, as the result of an
optimization for low miss ratio workloads, or as a result of a deliberate slowing down of a design in order
to hit a specific product price/performance point, as discussed below in section 6.
### 4.1. Effective Prefetching
An interesting characteristic of the IBM RS/6000 which can be observed in our measurements is
what we call effective prefetching. The cache does not have hardware support to do prefetching
[O’Bri90], but it can produce the same effect, that is, fetching cache lines before they are needed by the
computation, thus preventing the processor from stalling. This is accomplished in the RS/6000 by its
independent integer, branch, and floating-point units. In this respect the IBM RS/6000 behaves like a
decoupled architecture [Smit84, Good85, Wulf88]. The integer and branch unit can execute several
instructions ahead of the floating-point unit in floating-point intensive code and generate loads to the
cache that even in the presence of misses arrive before the floating-point unit requires the values
[O’Bri90]. Because the execution time of our test is dominated by floating-point operations, the illusion
of prefetching is present in our measurements. This is evident on the left side of the RS/6000 curves
(regime 2.a), independent of the address space region; as long as the stride is less or equal to 16 bytes (4
words), there is no miss penalty.
### 4.2. TLB Entries with Multiple Granularities
The results for the Sparcstation 1 and 1+ show that their respective TLB entry granularities are 128
Kbytes and more than 2 Mbytes. The reason for these large numbers is that the TLB entries can map
memory regions using four different levels of granularity. Furthermore, entries with different granularities can coexist in the TLB. The page table for these machines have four levels [Cypr90]. At each level,
there can be either a page table entry (PTE) or a page table pointer (PTP) which points to another page
table. A PTE can thus point to a region of 4GB (level 0), 16MB (level 1), 256KB (level 2) or 4KB (level
3). Each PTE in the TLB is tagged to indicate the size of the region it covers, and translation is done
accordingly. The operating system determines the coverage of a PTE at the time the region is mapped.
The availability of variable granularity PTEs has a number of advantages; in particular, it allows very
large memories to be referenced by small TLBs.
### 5. The Effect of Locality in the SPEC Benchmarks
In this section we combine the experimental cache and TLB results obtained in the last section with
the cache and TLB miss ratios for the Fortran SPEC benchmarks to compute the memory delay caused by
misses. We then use these results to evaluate: 1) whether our execution time predictions improve when
we incorporate the memory delay experience by the programs; and 2) how much impact does each cache
and TLB configuration have on the overall performance of their respective machines.
