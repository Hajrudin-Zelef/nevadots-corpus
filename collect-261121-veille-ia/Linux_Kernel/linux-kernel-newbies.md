---
id: collect-261121-veille-ia/linux-kernel/linux-kernel-newbies
title: "Linux Kernel Newbies"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["scheduler", "cache", "swap", "linux", "data", "kernel", "better", "scheduling"]
source: https://kernelnewbies.org/Linux_7.2
source_anchor: ""
source_lines: [1, 986]
section: "Linux_Kernel"
---
Linux 7.2 was released on Sunday, 16 August 2026.

Summary: Linux 7.2 modifies the task scheduler to be more cache-aware and is able to co-locate tasks that share data within the same Last Level Cache domain. It also adds a fairer GPU scheduler for better scheduling of GPU jobs; better memory reclaiming that can provides large performance increases in some situations; more performant and leaner swapping implementation; support for USB4STREAM, which allows to send data streams through USB4 cables; performance improvements in Btrfs; a new dm-inlinecrypt storage target for inline block device encryption: new flags for the openat(2) system calls; faster reading of /proc/filesystems and /proc/interrupts; and preparations for the introduction of sub-schedulers in sched_ext. As always, there are many other features, new drivers, improvements and fixes.

You might also be interested in the list of changes done by LWN: part 1, part 2

# 1. Prominent features


## 1.1. Faster performance and less cache misses with cache-aware task scheduling

This release introduces cache-aware load balancing of tasks, with the goal of co-locating tasks that share data (ie. threads of the same process) within the same Last Level Cache domain. By improving cache locality, the scheduler can reduce cache bouncing and cache misses, ultimately improving data access efficiency.

Recommended LWN article: Cache awareness for the CPU scheduler

Cover email: Cache aware scheduling


## 1.2. Fair(er) GPU scheduler

The Linux kernel has a task scheduler that schedules processes, but this is not the only scheduler in the kernel. Processes that use graphic card resources do submit jobs that are executed inside the GPU, and there is a GPU scheduler that decides which job will be executed next when the GPU has processing power available. The algorithm used by the GPU scheduler can impact the fairness and latency seen by the processes submitting these jobs, and right now the algorithm used is based around the "First In, First Out" principle, which has a number of disadvantages.

This release implements a "Fair(er)" GPU scheduler modeled after the ideas of the original Linux CFS task scheduler. There are improvements in fairness and scheduling of interactive clients when running in parallel with a heavy GPU load, and so far no scheduling regressions have been found relative to FIFO.

Recommended blog post: Fair(er) DRM GPU scheduler


## 1.3. Better memory reclaiming, for better performance

Memory reclaiming, which is the part of the kernel that decides which parts of the memory should be sent to the swap or evicted from memory, has been improved. This release cleans up and slightly improve MGLRU's reclaim loop and dirty writeback handling. As a result, we can see an up to ~30% increase in some workloads like MongoDB with YCSB and a huge decrease in file refault, no swap involved. Other common benchmarks have no regression, and LOC is reduced, with less unexpected OOM, too.


## 1.4. Swap table phase IV

Phase IV of the swap table work (phase I, II, III)

This release unifies the allocation and charging of anonymous and shmem swap in folios, provides better synchronization, consolidates the metadata management, hence dropping the static array and map, and improves the

Recommended LWN article: Swap tables, flash-friendly swap, swap_ops, and more


## 1.5. USB4STREAM for streaming data with USB4

In addition to Thunderbolt networking, which was already supported, this release adds support for streaming data directly over a cable as well (USB4STREAM). It is very simple and basically just transfers raw packets from one host to another. The driver exposes `/dev/tbstreamX` devices on each side of the link that can be used to transfer data using regular filesystem operations such as `read(2)` and `write(2)`: 

```
# cat /dev/tbstream0
# echo hello > /dev/tbstream0
```
This can be useful in cases where network tooling is not available or just for existing applications like `dd` and `cat` that do not support sockets. This feature can be used at the same time with thunderbolt_net so 


## 1.6. Various Btrfs improvements

This release brings various improvements for the Btrfs file system:

- Enable large folios by default (they were experimental since 6.17), with no feature limitations
- Experimental support for huge folios (up to 2M)
- New `GET_CSUMS` ioctl to return raw checksums to userspace, can be used for mkfs and deduplication optimizations
- Performance improvements on sequential writes and direct IO


## 1.7. New dm-inlinecrypt target for inline block device encryption

This release introduces a new dm target, dm-inlinecrypt, to support inline block-device encryption. The implementation builds on the work previously done in Android’s dm-default-key, but intentionally drops passthrough support, as that functionality does not appear likely to be accepted upstream in the near future. With this limitation, dm-inlinecrypt is positioned as a practical replacement for dm-crypt, rather than a general passthrough


## 1.8. openat(2) extensions

This release adds a couple of flags to the `openat(2)` system calls: 

- `OPENAT2_REGULAR` flag, which refuses to open anything but regular files, protecting services from being redirected to fifos, device nodes, and friends
- Add `O_EMPTYPATH` flag to`openat(2)` /`openat2(2)` . If passed, an empty path string is accepted and`LOOKUP_EMPTY` is set at path resolution time, allowing to reopen the file behind the file descriptor directly


## 1.9. Speedup reading /proc/filesystems and /proc/interrupts

Performance when reading these two procfs files is not relevant for most applications, however some extreme users benefit from optimizing these files as much as possible, or are even used more often that you would expect (`/proc/filesystems` is read by libselinux and is linked into numerous frequently used programs, even simple ones like sed). This release optimizes the generation of these files. 


## 1.10. Preparations for the introduction of sched_ext sub-schedulers

This release adds infrastructure that will be used to support sub-schedulers in sched_ext. These will be used for use cases like, for example, using different sched_ext schedulers in different cgroups

Recommended LWN article: Sub-schedulers for sched_ext


# 2. Core (various)

- (FEATURED) Add `O_EMPTYPATH` to`openat(2)` /`openat2(2)` . If passed,`LOOKUP_EMPTY` is set at path resolution time (cover), commit, commit
- (FEATURED) `openat(2)` :`OPENAT2_REGULAR` flag support, which refuses to open anything but regular files, protecting services from being redirected to fifos, device nodes, and friends (cover), commit, commit
- Task scheduler 
  - (FEATURED) Cache aware scheduling, with the goal of co-locating tasks that share data within the same Last Level Cache (LLC) domain. By improving cache locality, the scheduler can reduce cache bouncing and cache misses (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit and Cache aware scheduling enhancements (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Optimized Donor Migration for Proxy Execution (cover), commit, commit, commit, commit, commit, commit
  - tick: Refactor idle cputime accounting (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - (FEATURED) sched_ext: Topological CPU IDs (cids): a dense, topology-ordered CPU numbering where the CPUs of a core, LLC, or NUMA node form contiguous ranges, so a topology unit becomes a (start, length) slice (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - sched_ext: Improve exit-time diagnostics (cover), commit, commit, commit
  - sched_ext: bitmaps windowed over a slice of cid space, so a sub-scheduler can track, for example, the idle cids of its shard without a full NR_CPUS cpumask (cover), commit, commit, commit
  - scx_qmap: Convert to BPF arena (cover), commit, commit, commit, commit
  - fair: attempts to improve SD_ASYM_CPUCAPACITY scheduling by introducing SMT awareness (cover), commit, commit, commit, commit, commit
- io_uring 
  - zcrx: Add a mechanism to communicate conditions back to userspace via a dedicated CQE, with the initial users being notification on running out of buffers and on a frag copy fallback, plus shared-memory notification statistics (cover), commit, commit, commit, commit
  - net: support registered buffer for plain send and recv commit
  - net: allow filtering on `IORING_OP_CONNECT` commit
- pipe: reduce pipe->mutex contention by pre-allocating outside the lock. On a writers x readers sweep with 64KB writes against a 1 MB pipe throughput improves 6-28% and average write latency drops 5-22%; under memory pressure - when the cost of holding the mutex across reclaim is highest - throughput improves 21-48% and latency drops 17-33% (cover), commit, commit
- Improve write performance with `RWF_DONTCACHE` (cover), commit, commit, commit
- proc: `subset=pid` : Relax check of mount visibility (cover), commit, commit, commit, commit, commit, commit, commit
- Simple xattr improvements (cover), commit, commit, commit, commit
- exec: relocate the dumpable mode and the user_namespace captured at execve() from mm_struct onto a new per-task task_exec_state structure that stays attached to the task for its full lifetime (cover), commit, commit, commit, commit
- futex: Address the robust futex unlock race for real (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- writeback: fix race between cgroup_writeback_umount() and inode_switch_wbs() (cover), commit, commit, commit
- fanotify: lift pidfd reporting restrictions and allow fanotify to report pidfds referring to the event-generating thread (cover), commit, commit
- procfs 
  - (FEATURED Speedup reading `/proc/filesystems` (it is read frequently because libselinux reads it and is linked into numerous frequently used programs, even simple ones like sed) (cover), commit, commit, commit
  - (FEATURED Improve `/proc/interrupts` (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- VFS 
  - Add the vfs infrastructure required to implement fs-verity support with a post-EOF merkle tree: fsverity generates and stores a zero-block hash, and iomap learns to verify data on buffered reads commit, commit, commit, commit, commit
  - iomap: Skip the memset in iomap_iter() once the iteration is done. In high-IOPS scenarios (4k randread NVMe polling via io_uring) the pointless memset wasted memory write bandwidth; this improves IOPS by about 5% on ext4 and xfs commit
  - Prerequisites for supporting directory delegations in nfsd via CB_NOTIFY callbacks (cover), commit, commit, commit, commit, commit, commit, commit
  - Introduce `IOMAP_F_ZERO_TAIL` for filesystems that maintain a separate valid data length (exFAT, NTFS) commit, commit, commit, commit
  - Assorted dcache cleanups and fixes (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Assorted ->i_count changes + extension of lockless handling (cover), commit, commit, commit
- locking:
- liveupdate
- build 
  - objtool/klp: Calculate object checksums (cover), commit, commit, commit, commit, commit, commit
  - Distributed build support for clang thinlto (cover), commit, commit
  - rust: Add support for software tag-based kasan (cover), commit, commit
  - rust: add AutoFDO support commit
  - Rust gpuvm immediate mode (cover), commit, commit, commit, commit, commit
  - Remove the architecture specific config for autofdo and propeller (cover), commit, commit
  - kcov: Allow simultaneous kcov_enable/kcov_remote_enable commit
  - rust: zerocopy-derive: import crate commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- tools/nolibc 
  - Large file support commit, commit, commit, commit, commit, commit, commit
  - Add support for OpenRISC / or1k commit
  - Add alloca() commit
  - Add assert() and assert.h commit
  - Add err.h commit, commit, commit, commit
  - Add support for 32-bit parisc (cover), commit, commit
  - Bump required Clang version to 23 commit
- Remove strncpy() from the kernel commit


# 3. File systems

- BTRFS (FEATURED) 
  - Enable large folios by default (they were experimental since 6.17), no feature limitations commit
  - Support huge data folios for 4K page size (cover), commit, commit, commit, commit, commit
  - Add ioctl `GET_CSUMS` to read raw checksums from file range. It can be used for mkfs and deduplication optimizations commit
  - Limit size of bios submitted from writeback (reported improvement 15% on sequential writes) commit
  - Don't force dio writes to be serialized (~59% improvement in DIO throughput) commit
  - Lockless calculation of number of shrinkable extent maps, improve performance with many memcg allocated objects commit
  - Properly enable compressed readahead for bs < ps and large folio cases commit, commit
  - Optimize fill_holes() to merge a new hole with both adjacent items: an benefit workloads with heavily fragmented hole patterns commit
  - Provide stable UUID for e.g. overlayfs and temp_fsid, also reflected in statvfs() field f_fsid, internal dev_t is hashed in to allow cloning (cover), commit
  - Add 32-bit compat ioctl for `BTRFS_IOC_GET_SUBVOL_INFO` commit
  - Remove 32bit compat code for vfs inode number commit
  - cleanup: remove cow fixup mechanism and checked folio flag (cover), commit, commit
  - cleanup: reduce memory consumption for block groups (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - tree-checker: enhanced fst extent/bitmap checks commit, commit, commit
- EXT4 
  - A major rework of the fast commit mechanism to avoid lock contention and deadlocks. We also export snapshot statistics in /proc/fs/ext4/*/fc_info commit, commit, commit, commit, commit, commit, commit
  - Performance optimization for directory hash computation by processing input in 4-byte chunks and removing function pointers (cover), commit, commit
- XFS 
  - Drop the experimental warning for the zoned allocator commit
- F2FS
- NFSD 
  - Implement directory delegations (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Automatic NFSv4 state revocation on filesystem unmount (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Expose the case folding behavior of local filesystems so that file servers - nfsd, ksmbd, and user space file servers - can report the actual behavior to clients instead of guessing (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Add support for netlink upcalls for the auth.unix.ip and auth.unix.gid caches in SunRPC for mountd/exportd (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - The default NFS r/w block size rises to 4MB on hosts with at least 16GB of RAM, reducing per-RPC overhead on fast networks. Smaller machines keep their previously computed default, and the value remains tunable through /proc/fs/nfsd/max_block_size commit
  - Expose `FMODE_NOWAIT` for read-only files commit
- NTFS
- SMB
- CEPH
- EROFS
- EXFAT
- EXT2 
  - Remove deprecated dax support commit
- 9P


# 4. Memory management

- MGLRU 
  - (FEATURED) Clean up and slightly improve MGLRU's reclaim loop and dirty writeback handling. As a result, we can see an up to ~30% increase in some workloads like MongoDB with YCSB and a huge decrease in file refault, no swap involved. Other common benchmarks have no regression, and LOC is reduced, with less unexpected OOM, too (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Use folio_mark_accessed to replace folio_set_active commit
- (FEATURED) Swap table phase IV: unify allocation and reduce static metadata: Unify the allocation and charging of anon and shmem swap in folios, provides better synchronization, consolidates the metadata management, hence dropping the static array and map, and improves performance (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- khugepaged: support the ability to collapse anonymous memory regions to mTHPs (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Speed up the freeing of a batch of 0-order pages by first scanning them for coalescing opportunities. This is applicable to vfree() and to the releasing of frozen pages (cover), commit, commit, commit
- filemap: tighten mmap_miss hit accounting. This fixes a flaw where the mmap_miss counter over-credited page cache hits during fault-arounds and page-fault retries. This results in significant reduction of redundant synchronous mmap readahead I/O, drastically cutting down execution time and gigabytes read for sparse random or strided memory access workloads (cover), commit, commit
- Remove `CONFIG_READ_ONLY_THP_FOR_FS` and enable file THP for writable files (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Shrink obj_stock_pcp and cache multiple objcgs: Resolve a 68% performance regression caused by NUMA-node cache thrashing (cover), commit, commit, commit, commit
- Remove b_end_io from struct buffer_head (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- `/proc/meminfo` : expose per-node balloon pages in node meminfo commit
- Improve large-folio readahead on systems like 64K-page arm64 by preventing the mmap_miss check from permanently disabling target-oriented VM_EXEC readahead, and by generalizing the force_thp_readahead gate to support mappings with any usefully large maximum folio order under the cache cap (cover), commit, commit
- lruvec: preemptively free dead folios during lru_add drain commit
- Use per-vma locks for /proc/pid/maps reads (cover), commit, commit, commit, commit, commit, commit
- page_alloc: replace kernel_init_pages() with batch page clearing commit
- sparse: Remove sparse buffer pre-allocation mechanism commit
- vmalloc: free unused pages on vrealloc() shrink when shrinking across a page boundary (cover), commit, commit, commit, commit, commit
- vmpressure: skip socket pressure for costly order reclaim commit
- Switch THP shrinker to list_lru (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- slab: support for compiler-assisted type-based slab cache partitioning (cover), commit, commit, commit
- slab: introduce alloc_flags and slab_alloc_context (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- slub: defer freelist construction until after bulk allocation from a new slab. This reduces per-object overhead in bulk allocation paths and improves allocation throughput significantly commit
- slub: batch partial slab list operations (cover), commit, commit
- damon 
  - Let DAMON be paused and resumed (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - repost non-hotfix reviewed patches in damon/next tree (cover), commit, commit, commit, commit
  - Introduce DAMOS failed region quota charge ratio commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Introduce data attributes monitoring commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Docs/mm/damon/design: document data attributes monitoring (cover), commit, commit, commit, commit, commit, commit, commit, commit
  - reclaim: add autotune_monitoring_intervals parameter commit, commit
  - stat: add kdamond_pid parameter commit, commit
  - Add node_eligible_mem_bp goal metric commit
- hw_tags: Disable tagging for stack and page-tables (cover), commit, commit, commit
- dma-buf: heaps: Turn heaps into modules (cover), commit, commit, commit, commit


# 5. Block layer

- Enable lock context analysis for the block layer core (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- dm cache: make smq background work limit configurable commit
- (FEATURED) dm-inlinecrypt: add target for inline block device encryption (cover), commit, commit, commit
- dm-inlinecrypt: add support for hardware-wrapped keys (cover), commit


# 6. Tracing, perf and BPF

- BPF 
  - arena: Recover from BPF arena page faults using a scratch page and add ptep_try_set() for lockless empty-slot installs on x86 and arm64 commit, commit, commit, commit, commit, commit, commit, commit
  - Raise maximum call chain depth to 16 frames commit
  - Support stack arguments for BPF functions and kfuncs (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Adding tracing_multi link support that allows fast attachment of tracing program to many functions commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Introduce resizable hash map (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Maximum combined stack depth (cover), commit, commit, commit
  - Minimize annotations for arena programs (cover), commit, commit, commit, commit, commit, commit
  - Add bpf_real_inode() kfunc commit
  - Support BPF traversal of wakeup sources (cover), commit, commit
  - Add simple xattr support to bpffs commit
  - Add support for sleepable tracepoint programs (cover), commit, commit, commit, commit, commit, commit
  - Extend bpf syscall with common attributes support (cover), commit, commit, commit, commit, commit, commit, commit, commit
  - Expose signature verdict via bpf_prog_aux commit, commit
  - cBPF JIT spray hardening commit, commit, commit, commit, commit, commit
  - Extend the bpf_list family of apis (cover), commit, commit, commit, commit, commit, commit, commit, commit

- tracing/eprobes: Allow use of BTF names to dereference pointers. Add syntax to the parsing of eprobes to be able to typecast a trace event field that is a pointer to a structure commit
- perf 
  - Allow perf to read synthetic events commit
  - perf tools: Add inject `--aslr` feature, to remap ASLR-randomized addresses in perf.data files, enabling reproducible analysis across runs with different address space layouts (cover), commit, commit, commit, commit, commit
  - perf bench: add `--write-size` option to sched pipe commit
  - perf lock contention: Allow 'mmap_lock' in `-L` /`--lock-filter` commit
  - perf test: Parallel harness optimizations, summary, JUnit XML & PMU fixes commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - perf trace: Introduce `--show-cpu` option to display cpu id commit
  - Perf libunwind multiple remote support (cover), commit, commit, commit, commit, commit, commit, commit
  - perf: Reveal PMU type in fdinfo commit


# 7. Virtualization

- virtio
- SR-IOV Virtual Function support for AIE4 platform (cover), commit, commit, commit, commit, commit, commit


# 8. Cryptography

- AF_ALG: Remove support for AIO and old-style drivers commit, commit, commit
- AF_ALG: remove zero-copy support from skcipher and aead commit
- Fix and simplify the NIST DRBG implementation (cover), commit, commit, commit, commit, commit, commit, commit, commit
- Consolidate FCrypt and PCBC code into net/rxrpc/ (cover), commit, commit, commit, commit, commit


# 9. Security

- integrity: Add support for ML-DSA signature for EVM and IMA (cover), commit, commit, commit, commit
- ima: support for removing IMA measurement list records stored in kernel memory (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- landlock: Account all audit data allocations to user space commit
- landlock: Implement LANDLOCK_ADD_RULE_QUIET to mute specific specific audit logs (and other future observability events) (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- landlock: add new Landlock access rights to control UDP bind and connect/send operations (cover), commit, commit, commit, commit, commit, commit
- KEYS: trusted: Debugging as a feature commit
- apparmor: Enable differential encoding commit


# 10. Networking

- IPv4: nexthop removal performance optimization commit, commit, commit
- TCP: rehash onto different local ECMP path on retransmit timeout (cover), commit, commit
- IPv6: Honor oif when choosing nexthop for locally generated traffic (cover), commit, commit, commit
- IPv6: don't use rtnl_lock for multicast routing configuration (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- `SO_TXTIME` improvements (cover), commit, commit, commit
- mptcp: pm: drop TCP TS with ADD_ADDRv6 + port (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Reimplement TCP-AO using crypto library (cover), commit, commit, commit, commit, commit
- Expose page pool order via netlink (cover), commit, commit
- pppoe: implement GRO/GSO support commit, commit
- geneve: Allow binding UDP socket to a specific address commit, commit, commit, commit
- RDMA: Introduce generic buffer descriptor infrastructure for umem (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Add ovs packet family ynl spec and unicast notification support (cover), commit, commit
- Remove tls_toe (cover), commit, commit
- bridge
- Avoid nested UP notifier events commit, commit, commit
- sched: tc_dump_qdisc() optimizations (cover), commit, commit, commit, commit
- wifi 
  - Introduce Fine Timing Measurement (FTM) enhancements and Proximity Detection (PD) capabilities. The implementation follows the Wi-Fi Alliance "PR Implementation Consideration Draft 1.9 Rev 1" specification and extends the existing peer measurement framework with advanced ranging features (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Indicate (Re)Association frame encryption in SME-in-driver mode (cover), commit, commit
  - mlme: advertise driver's extended MLD capa/ops commit
  - UHR non-primary channel access commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Add 802.3 multicast encapsulation offload support (cover), commit, commit, commit
  - mac80211_hwsim: split NAN handling into separate file (cover), commit, commit, commit, commit, commit
  - More NAN patches commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Basic S1G rx rate reporting support commit
  - cfg80211: remove 5/10 MHz channel support commit
- SUNRPC: Switch MIC token verification to crypto/krb5 (cover), commit, commit, commit, commit, commit, commit, commit, commit
- appletalk: move the protocol out of tree commit, commit
- atm: Remove more dead code (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- hsr: Broadcast netlink notifications in the device's net namespace commit
- netfilter
- sched: netem: enhancements (cover), commit, commit, commit, commit, commit
- devmem: support devmem with netkit devices (cover), commit, commit, commit, commit, commit, commit, commit, commit
- flowtable fixes for ipip tunnels commit, commit, commit
- psp: Add support for dev-assoc/disassoc commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- xfrm: XFRM_MSG_MIGRATE_STATE new netlink message (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- xprtrdma: Decouple req recycling from RPC completion (cover), commit, commit, commit, commit, commit
- cgroup/rdma: add rdma.peak and rdma.events[.local] (cover), commit, commit, commit, commit
- ethtool: let ops locked drivers run without rtnl_lock (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Bluetooth
- ARCnet: remove outdated drivers and information and unused code (cover), commit, commit, commit, commit, commit, commit, commit


# 11. Architectures

- ARM 
  - Device Tree Sources 
    - New SoC Qualcomm Dragonwing IPQ9650, is a new wireless networking SoC using four Cortex-A55 and one Cortex-A78 core, which is a significant upgrade from older generations (cover), commit, commit, commit, commit
    - New SoC ZTE zx297520v3, an older low-end wireless SoC using a single Cortex-A53 core, which so far can only run 32-bit kernels. This brings back the ZX family of chips that was removed in 2021 after support for the original zx296702 and zx296718 chips was never completed (cover), commit, commit, commit, commit
    - New SoC Renesas R-Car M3Le (R8A779MD), a variant of the R-Car M3-N (R8A77965) automotive SoC commit
    - New SoC Apple t8122 (M3), the 2023 generation of their laptop SoCs, which has now been reverse-engineered to the point of having initial kernel support for five laptop models (cover), commit, commit, commit, commit, commit
    - New SoC ASPEED AST27xx, their first baseboard managment controller using a 64-bit core, the Cortex-A35, following earlier generations using ARMv5/v6/v7 CPUs (cover), commit, commit, commit, commit
    - Two NAS boxes using the old Cortina Systems Gemini SoC based on an ARMv4 FA526 CPU core (cover), commit, commit, commit, commit, commit, commit, commit
    - 18 industrial embedded boards using NXP i.MX6/8/9 and LX2160A ((cover), commit, commit, commit, commit, commit, commit, commit, commit) SoCs from Variscite ((cover), commit, commit, commit), , Toradex ((cover), commit, commit, commit) and SolidRun ((cover), commit, commit), plus a number of overlays for combinations with additional boards (cover), commit, commit, commit
    - One new carrier board and SoM using TI K3 AM62x, in addition to new overlays for older SoMs (cover), commit, commit
    - Three phones from Google ((cover), commit, commit), Nothing ((cover), commit, commit, commit) and Motorola (commit, commit), all using Qualcomm Snapdragon SoCs
    - AST26xx BMC support for two server boards, (cover), commit, commit, (cover), commit, commit
    - Add initial support for rk3528 (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
    - Add interconnect support for qualcomm nord soc (cover), commit, commit
    - qcom: Add support for upcoming Hawi SoC (cover), commit, commit
    - qcom: Add support for the microsoft surface pro 12in 1st edition (snapdragon), (cover), commit, commit, commit, commit, commit
    - qcom: pd-mapper: Add support for Hawi SoC commit
    - Add initial device trees for nord SA8797P (cover), commit, commit, commit
    - Enable new features for flipkart-rimob (cover), commit, commit
    - Basic devicetree support for amlogic a9 (cover), commit, commit
    - Add i.mx91 9x9 qsb basic support (cover), commit, commit, commit, commit, commit
    - Add nxp frdm-imx95-pro board support (cover), commit, commit
    - Support tqma8qm (cover), commit, commit
    - Add verdin imx8m[mp] and imx95 zinnia board (cover), commit, commit, commit, commit, commit, commit, commit
    - imx8mp: Add DT overlays for DH i.MX8M Plus DHCOM SoM and boards commit
    - socfpga: agilex7m: Add SoCFPGA Agilex7-M devkit commit, commit
    - Add interconnect support for qualcomm shikra soc (cover), commit, commit
    - amlogic: T7 thermal support (cover), commit, commit, commit, commit
    - samsung: Add support for Google GS101 TMU (cover), commit, commit, commit, commit, commit, commit
    - stratix10-svc: Add support to query Arm Trusted Firmware (ATF) version commit
    - tegra: bpmp: Add support for multi-socket platforms commit, commit
    - Add efinix fpga configuration support (cover), commit, commit, commit
    - gpib: Add support for ines pci_xl board commit, commit, commit, commit, commit, commit, commit
    - mtk: add support for hw access via SMCC commit, commit, commit
    - Ast2700-a2 interrupt controller hierarchy and route support (cover), commit, commit, commit, commit
    - Add support for the ti bq25792 battery charger (cover), commit, commit, commit, commit, commit, commit, commit, commit
    - tegra: pmc: Add Tegra238 support commit, commit
    - renesas: Add R-Car X5H PRR support commit
    - soc: renesas: add MFIS driver (cover), commit
    - renesas: rcar-mfis: Add R-Car V4H/V4M support commit
    - sunxi: sram: Add H616 SRAM regions commit
    - Add am62p silicon revision detection via nvmem (cover), commit, commit
    - pmic-arb: Add spmi-pmic-arb support for Qualcomm Hawi SoC (cover), commit, commit
    - Add cpucp mailbox support for qualcomm nord soc (cover), commit, commit
    - rkisp1: Add support for CAC commit
    - Tegra114: implement EMC support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
    - tegra: Add Tegra238 MC support commit
    - Add support for verisilicon iommu used by media codec blocks (cover), commit, commit, commit, commit
    - Add driver for ec found on qualcomm reference devices (cover), commit, commit
    - qcom: rpmpd: Add support for Shikra (cover), commit, commit
    - Add ufs support for exynosautov920 soc (cover), commit, commit, commit
  - xor: Replace vectorized version with intrinsics (cover), commit, commit, commit, commit
  - cpufeature: Add WORKAROUND_DISABLE_CNP capability (cover), commit, commit
  - Add support for the mpam v0.1 architecture version (cover), commit, commit, commit, commit
  - errata: Mitigate TLBI errata on Microsoft Azure Cobalt 100 CPU (cover), commit, commit, commit, commit, commit, commit, commit, commit
  - Unmap linear alias of kernel data/bss (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - arm64+KVM: FPSIMD/SVE/SME cleanups (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Device Tree Sources 
- X86 
  - Remove support for TSC-less and CX8-less CPUs commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Add a `tlbi=` cmdline switch commit
  - platform 
    - amd pmc: Delay s2idle suspend for some devices (cover), commit, commit, commit, commit
    - amd/pmc: Add support for AMD 1Ah Family 80h SoC (cover), commit, commit
    - intel-uncore-freq: Expose instance ID in the sysfs (cover), commit, commit
    - asus-armoury: more ppt data (cover), commit, commit, commit
    - asus-wmi: add keystone dongle support commit
    - dell-dw5826e: Add reset driver for DW5826e commit
    - dell-laptop: add Inspiron N5110 to touchpad LED quirk table commit
    - hp-wmi: Add support for Omen 16-ap0xxx (8D26) commit
    - hp-wmi: Add support for Omen 16-ap0xxx (8E35) commit
    - hp-wmi: Add thermal support for board 8B2F commit
    - intel-hid: Add HP ProBook x360 440 G1 to button_array_table commit, (cover)
    - lenovo-wmi: Add fixes and enhancement (cover), commit, commit, commit, commit, commit, commit, commit
    - msi-ec: Add support for MSI Pulse GL66 12th Gen commit
    - oxpec: add support for OneXPlayer Super X commit
    - thinkpad_acpi: Add debugfs entry to display HWDD raw commit
    - uniwill-laptop: Charging-related improvements (cover), commit, commit, commit, commit, commit, commit, commit, commit
    - Add acpi-based pmt discovery support for intel pmc (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
    - Enable nvl support in intel_pmc_core (cover), commit, commit, commit, commit, commit, commit, commit
    - Add tps68470 power supply support for ov13b10 sensor (cover), commit, commit, commit
    - Add support for hygon dhyana family 18h processor (cover), commit
  - KVM 
    - Combined patchset for MBEC/GMET support. Both MBEC and GMET allow more granular control over execute permissions, with different levels of separation between supervisor and user mode. MBEC provides support for separate supervisor and user-mode bits in the PTEs; GMET instead lacks supervisor-mode only execution commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
    - pmu: Add support for AMD's Host-Only and Guest-Only performance counter eventsel bits in KVM's mediated PMU passthrough implementation (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
    - Extend `KVM_HC_MAP_GPA_RANGE` api to allow retry (cover), commit, commit
    - Virtualize AMD's "disable CPUID in usermode" (cover), commit, commit, commit, commit, commit
  - Runtime TDX module update support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - cpuid: Introduce a centralized CPUID parser commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - topology: Add paramter to split LLC commit
  - cpu: Add Intel CPU model number for rugged Panther Lake commit
  - Add IOMMU TLB and interrupt metrics (cover), commit, commit
  - perf vendor events intel: update (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- S390 
  - Add support for 2G hugepages (cover), commit, commit, commit, commit
  - Introducing kvm_arch_set_irq_inatomic Fast Inject (cover), commit, commit, commit
  - Implement KVM_PRE_FAULT_MEMORY (cover), commit, commit, commit, commit, commit
  - vsie: Implement ASTFLEIE facility 2 (cover), commit, commit, commit, commit
  - Enable rust support and add required arch glue (cover), commit, commit, commit, commit, commit, commit
  - Convert various functions to C (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Improve this_cpu operations (cover), commit, commit, commit, commit, commit, commit, commit
  - ap: Implement SE bind and associate uevents commit
  - debug: Add s390dbf kernel parameter commit
  - sclp: Allow user-space to provide PCI reports for NVMe SMART data commit
  - tracing: Add s390-tod clock commit
- RISCV 
  - Support non-leaf and range invalidation features in risc-v (cover), commit, commit, commit, commit, commit, commit, commit, commit
  - k3: Add support for CoM260-IFX board (cover), commit, commit
  - Add deepcomputing fml13v05 board dts (cover), commit, commit
  - KVM: Batch stage-2 TLB flushes commit
  - Implement ARCH_HAS_CC_CAN_LINK commit
- LOONGARCH
- POWERPC
- SH 
  - sh: ecovec24: remove FSI/DA7210/Simple-Audio-Card support commit


# 12. Drivers


## 12.1. Graphics

- (FEATURED) Fair(er) DRM scheduler. It improves fairness and scheduling of interactive clients when running in parallel with a heavy GPU load (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- xe 
  - Add initial CRI platform support commit
  - xe3p: add gt tuning TileY 2x2 walk pattern commit
  - xe_sysctrl: Add system controller component for Xe3p dGPU platforms (cover), commit, commit, commit, commit, commit, commit, commit
  - Expose multi_lrc_mask in debugfs info (cover), commit, commit
  - PF fair scheduling auto provisioning commit
- amdgpu 
  - Initial HDMI 2.1 FRL support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Initial DCN 4.2.1 support commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - GC 11.5.6/SDMA 6.4.0/and other new IPs commit, commit, commit, commit, commit, commit, commit
  - Enable DC power module commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Finish support for using multiple SDMA queues for TTM operations commit, commit, commit, commit, commit, commit
  - Add amdgpu.ptl module parameter for PTL control (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Auxless ALPM support (cover), commit, commit, commit, commit, commit, commit, commit, commit
  - Add guest driver CUID support commit
  - amdkfd: Add kfd_ioctl_profiler to contain profiler kernel driver changes commit
- i915
- panels 
  - Panel replay bw optimization (cover), commit, commit, commit
  - simple: add Waveshare LCD panels (cover), commit, commit
  - Add panel driver for ChipWealth CH13726A based panels (cover), commit, commit
  - Support Waveshare DSI TOUCH kits (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - simple: Add AM-1280800W8TZQW-T00H commit, commit
  - panel-edp: Add AUO B133HAN06.6 and BOE NV133FHM-N4F V8.0 commit
  - panel-edp: Add and update multiple AUO, BOE, CMN, and IVO panels (cover), commit, commit, commit, commit
  - panel-edp: Add CSW PNB601LS1-2 and LGD LP116WHA-SPB1 commit
  - simple: Add Displaytech DT050BTFT-PTS panel commit
  - simple: Displaytech DT050BTFT-PTS panel commit
  - simple: Add Startek KD070HDFLD092 panel commit
  - Add support for TSD TST070WSBE-196C 8" MIPI-DSI panel (cover), commit, commit
  - Add nec lcd technologies nl6448bc33-70c panel support (cover), commit, commit
- bridge
- msm 
  - Add PERFCNTR_CONFIG ioctl (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Add support for adreno 810 gpu (cover), commit, commit, commit, commit, commit, commit
  - Mdss / dpu bringup for milos (cover), commit, commit, commit, commit, commit, commit, commit
- nouveau
- Add rz/g3l gfx support (cover), commit, commit, commit, commit
- panthor
- sysfb: Support basic power management commit, commit, commit, commit, commit
- tyr
- v3d
- verisilicon
- nova-core 
  - firmware: Hopper/Blackwell support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
  - Add GA100 support (cover), commit, commit, commit, commit, commit, commit
  - Run unload sequence upon unbinding (cover), commit, commit, commit, commit
- drm_ras
- gud 
  - Add RCade Display Adapter VID/PID pair commit
- console: mdacon: remove this obsolete driver commit
- fbdev: remove Hercules monochrome ISA graphics adapter driver commit
- Add new max25014 backlight driver (cover), commit, commit, commit, commit
- Add new pixel formats for xilinx zynqmp (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Add du support for rz/t2h and rz/n2h socs (cover), commit, commit, commit


## 12.2. Power Management

- CPPC: Add CPPC v4 support (ACPI 6.6) (cover), commit
- Add lecarc acpi ids for designware gpio, spi, i2c (cover), commit, commit, commit
- thermal/drivers/amlogic: Add support for secure monitor calibration readout commit
- thermal/drivers/qcom/tsens: Atomic temperature read with hardware-guided retries commit
- thermal/drivers/qoriq: Add i.MX93 tmu support commit
- thermal/drivers/samsung: Enable TMU by default commit
- thermal/drivers/spacemit/k1: Add thermal sensor support commit
- Support cooling device with ID in the OF commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- intel: intel_tcc_cooling: Add Arrow Lake CPU models commit


## 12.3. Storage

- nvme: Enable PCI P2PDMA support for RAID0 and NVMe Multipath (cover), commit, commit, commit
- nvme: export additional diagnostic counters via sysfs (cover), commit, commit, commit, commit, commit, commit, commit, commit
- nvme: enable passthrough iostats (cover), commit, commit
- nvme: Refactor and expose per-controller timeout configuration (cover), commit, commit, commit, commit, commit, commit, commit
- scsi: ufs: Add persistent TX Equalization settings support (cover), commit, commit


## 12.4. Drivers in the Staging area

- atmel-isc: Remove driver commit


## 12.5. Networking

- Bluetooth 
  - btmtk: add event filter to filter specific event commit
  - btusb: Add Realtek RTL8922AE VID/PID 0bda/d922 commit
  - btusb: Add Realtek RTL8922AE VID/PID 0bda/d923 commit
  - btusb: Add TP-Link UB600 for Realtek 8761BUV commit
  - btusb: Add USB ID 2c4e:0128 for Mercusys MA60XNB commit
  - btusb: Add support for Intel Lizard Peak 2 (0x8087:0x0040) commit
  - btusb: Add support for TP-Link TL-UB250 commit, (cover)
  - btusb: MT7922: Add VID/PID 0e8d/223c (cover), commit, commit
  - btusb: MT7925: Add VID/PID 13d3/3609 commit
- RDMA
- Support perf mgmt for rxe (cover), commit, commit, commit, commit
- b43: Complete n-phy rev 8 + radio 2057 rev 8 support (cover), commit, commit, commit, commit, commit, commit, commit
- virtio: Add virtio CAN driver commit
- Add basic driver framework for alibaba elastic ethernet adaptor (cover), commit, commit, commit, commit, commit, commit, commit, commit
- gve: Add support for ptp gettimex64 (cover), commit, commit, commit
- ice: Add tx reference clock index handling to an restart command (cover), commit, commit, commit
- dpll/ice: Add generic DPLL type and full TX reference clock control for E825 commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Enable eee for e610 devices (cover), commit, commit, commit, commit, commit, commit
- spectrum_ethtool: expose per-PG rx_discards commit
- mv88e6xxx: SERDES on mv88e6321 (cover), commit, commit, commit
- mlx5: Support state get/set for satellite PF ports (cover), commit, commit
- mlx5: ICM page management in VHCA_ID mode (cover), commit, commit, commit
- mlx5: Add satellite PF support commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- mlx5: Add vhca_id_type support to IPsec alias creation commit
- mlx5: Add switchdev mode support for Socket Direct single netdev, part 1/2 (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- mlx5: Add switchdev mode support for Socket Direct single netdev, part 2/2 (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- mlx5: enable sub-page allocations for mlx5_frag_buf (cover), commit, commit, commit
- mlx5e: Report more netdev stats (cover), commit, commit, commit, commit, commit
- mlx5e: improve RSS indirection table sizing and resizing commit, commit, commit, commit, commit
- devlink, mlx5: Add new parameters for link management and SRIOV/eSwitch configurations (cover), commit, commit, commit, commit, commit
- airoha: Support multiple net_devices connected to the same GDM port (cover), commit, commit, commit, commit, commit, commit
- atlantic: add PTP support for AQC113 (Antigua) (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- atlantic: add AQC113 PTP traffic class and TX path setup commit
- ksz87xx: add support for low-loss cable equalizer errata (cover), commit, commit, commit
- dsa: mv88e6xxx: enable devlink ATU hash param for 6320 family (cover), commit, commit, commit
- dsa: netc: introduce NXP NETC switch driver for i.MX94 (cover), commit, commit, commit
- dsa: mv88e6xxx: enable devlink ATU hash param for 6320 family commit
- dsa: mxl862xx: SerDes ports (cover), commit, commit, commit, commit
- dsa: netc: introduce NXP NETC switch driver for i.MX94 (cover), commit, commit, commit, commit, commit
- dsa: netc: add bridge mode support commit, commit, commit, commit, commit, commit, commit, commit
- dsa: netc: introduce NXP NETC switch driver for i.MX94 commit
- dsa: qca8k: Add support for force mode for fixed link topology commit
- dsa: realtek: rtl8365mb: bridge offloading and VLAN support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- dsa: realtek: rtl8365mb: add support for RTL8367SB commit
- dsa: yt921x: Add ACL support (cover), commit, commit
- dsa: yt921x: Add port TBF support (cover), commit, commit, commit
- dsa: yt921x: Add port police support (cover), commit, commit, commit
- enetc: add CBDR setup/teardown hooks to enetc_si_ops for VF support (cover), commit, commit, commit
- Add preliminary NETC switch support for i.MX94 commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- hns3: enhance tc flow offload support commit, commit, commit, commit, commit, commit
- ibm: emac: Use napi_gro_receive() for Rx packets commit
- mana: Add support for PF device 0x00C1 commit
- mana: Per-vPort EQ and MSI-X management (cover), commit, commit, commit, commit, commit, commit
- mana: Expose hardware diagnostic info via debugfs commit
- mana: Optimize irq affinity for low vcpu configs commit
- mdio: realtek-rtl9300: Groundwork for multi SOC support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- mdio: realtek-rtl9300: Add RTL931x support (cover), commit, commit, commit, commit, commit
- Introduce airoha an8801r series gigabit ethernet phy driver (cover), commit, commit, commit, commit, commit, commit
- phy: dp83867: add MDI-X management commit
- phy: intel-xway: add PHY-level statistics via ethtool commit
- Add motorcomm 8531s set ds func and 8522 driver (cover), commit, commit, commit
- phy: realtek: Add support for PHY LEDs on RTL8221B commit
- phy: realtek: support MDI swapping for RTL8226-CG commit
- sfp: extend SMBus support (cover), commit, commit
- Add starfive jhb100 soc sgmii gmac support (cover), commit, commit, commit, commit
- Add meig srm813q ids in qmi_wwan and option (cover), commit, commit
- Add support for the rtl8159 10gbit usb ethernet chip (cover), commit, commit, commit
- Enable power management for Telit LE310X1 commit, commit
- ath12k: Add support for handling incumbent signal interference in 6 GHz (cover), commit, commit
- ath12k: Add support for 4-address mode commit
- ath12k: thermal throttling and cooling device support (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: extract NAN capabilities setting to a function (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: track TX/RX IGTKs separately (cover), commit, commit, commit, commit, commit
- iwlwifi: mark that we support iwl_rx_mpdu_desc version 7 and 8 (cover), commit, commit, commit, commit, commit
- iwlwifi: validate the channels received in iwl_mcc_update_resp_v* (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: add chan-load hysteresis for MLO scan triggers (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: update link grading tables per bandwidth (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: add NAN DATA support - part 1 commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- iwlwifi: mld: add link and link station FW IDs to debugfs (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: add link and link station FW IDs to debugfs commit
- iwlwifi: mld: implement UHR DPS commit
- iwlwifi: mld: implement UHR multi-link PM commit
- iwlwifi: mld: set fast-balance scan for active EMLSR (cover), commit, commit, commit, commit, commit
- iwlwifi: mld: support NPCA capability for UHR devices commit
- iwlwifi: mld: support the new statistics APIs commit
- mac80211_hwsim: add NAN data path TX/RX support (cover), commit, commit, commit, commit
- mac80211_hwsim: add NAN PHY capabilities commit
- mac80211_hwsim: add NAN data path TX/RX support commit
- mac80211_hwsim: switch to use TXQs (cover), commit, commit, commit, commit, commit, commit, commit, commit
- mt76: mt76x2u: Add support for ELECOM WDC-867SU3S commit
- mt76: mt7921: support per-device regulatory settings commit, commit, commit, commit, commit
- mt76: mt7921u: add MT7902 USB support commit
- mt76: mt7925: MT7927 (Filogic 380) support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- mt76: mt7925: add Netgear A8500 USB device ID commit
- mt76: mt792x: report txpower for the requested vif link commit, commit, commit
- rtw89: use firmware offload for PHY and RF batch register writes (cover), commit, commit, commit, commit, commit, commit
- rtw89: 8851bu: add Mercusys MA60XNB (2c4e:0128) commit
- rtw89: 8922d: change naming number and update values for WDE/PLE quota (cover), commit, commit, commit
- rtw89: debug: show large MRU in txpwr_table dbgfs commit
- rtw89: 8922d: configure TX shape settings commit
- rtw89: phy: increase RF calibration timeouts for USB transport (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- rtw89: Wi-Fi 7 configure TX power limit for large MRU commit
- rtw89: add debugfs entry of monitor mode options to capture HE-MU packets (cover), commit, commit, commit, commit
- rtw89: add USB IO offload and some refactors commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- rtw89: improve radiotap especially HE SIG-A/SIG-B commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- rtw89: debug: add RX statistics in bb_info commit
- rtw89: debug: add BB diagnose commit
- rtw89: debug: extend bb_info with TX status and PER commit
- rtw89: phy: support per PHY RX statistics commit
- rtw89: usb: Support switching to USB 3 mode commit
- rtw89: add dev_id_quirks to driver_info for per-device quirk control commit
- Remove all pcmcia drivers commit


## 12.6. Audio

- FCP: Add Focusrite ISA C8X support commit
- M-audio C600 disable output gain knob commit
- hda
- oxygen: add HT-Omega eClaro (7284:9783) support commit
- usb-audio: Add quirk for YAMAHA CDS3000 commit
- qcom: Add Eliza audio support (cover), commit, commit, commit
- Add es9356 focused soundwire codec (cover), commit, commit, commit, commit, commit, commit
- Improve sdca support for duplicated features (cover), commit, commit, commit
- SOF: Intel: lnl: Enable offload for UAOL link commit
- SOF: ipc4-topology: Enable deep buffer capture commit
- SOF: topology: allow user to add topologies (cover), commit, commit
- ASoC: AMD: ACP7.x initial PCI driver bring-up commit, commit, commit, commit, commit
- Add tas67524 quad-channel class-d amplifier driver (cover), commit, commit, commit, commit
- nau8822: add support for supply regulators (cover), commit, commit
- cs42xx8: Add SPI bus support for CS42448/CS42888 codec (cover), commit, commit
- fsl-asoc-card: Add some improvements (cover), commit, commit, commit, commit, commit
- fsl_sai: Add RX/TX BCLK swap support commit, commit
- imx-rpmsg: Add headphone jack detection and driver_name support (cover), commit, commit, commit
- mediatek: mt2701: HDMI audio support (cover), commit, commit, commit, commit, commit, commit
- mediatek: Add support for MT8196 SoC (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- qcom: qdsp6: add push/pull module support (cover), commit, commit, commit, commit, commit, commit
- rsnd: Add RZ/G3E audio driver support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- rt722-sdca: Add a control to support CAE firmware update commit
- rt722-sdca: add FU06 Playback Switch for speaker mute control commit
- Add support for gpios driven amplifiers (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- soc-core: Add core support for ignoring suspend on selected DAPM widgets (cover), commit, commit, commit
- spacemit: add K3 SoC support (cover), commit, commit
- Add ti tac5xx2 soundwire codec driver support (cover), commit, commit, commit, commit
- tas2781: Add TAS2573 support commit, commit
- tegra: Add S8 PCM format support for MVC commit
- tegra: Add per-stream Mixer Fade controls commit
- ti: davinci-mcasp: Add audio-graph-card2 and DPCM support commit
- tlv320aic3x: Add multi endpoint support commit


## 12.7. Tablets, touch screens, keyboards, mouses

- bpf: Add Huion Inspiroy Frego M button quirk commit
- Firmware support for usb-hid devices and cp2112 (cover), commit, commit
- Add onexplayer configuration hid driver (cover), commit, commit, commit, commit, commit
- lenovo: Use KEY_PERFORMANCE capability for ThinkPad X12 Tab Gen 2 commit
- logitech-hidpp: sync wheel multiplier on wheel mode changes commit
- nintendo: add support for HORI Wireless Switch Pad commit
- playstation: Add DualSense Edge extra button support commit
- rakk: add support for Rakk Dasig X side buttons commit
- Add support for wacom w9000-series penabled touchscreens (cover), commit, commit
- Pcap_ts: remove unused driver commit
- stmfts: support FTS5 (cover), commit, commit, commit, commit, commit, commit, commit
- xpad: add support for ZENAIM LEVERLESS commit
- Xilinx_ps2: remove driver commit


## 12.8. TV tuners, webcams, video capturers

- Wave5: Add encoder features (cover), commit, commit, commit, commit
- cadence,ti: CSI2RX Multistream Support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- cec: add CEC LIP support (cover), commit, commit, commit, commit
- i2c: cvs: Add Intel CVS driver (cover), commit, commit, commit
- iris: add support for purwa platform (cover), commit, commit
- iris: add FPS calculation and VPP FW overhead in frequency formula commit
- pci: add AVMatrix HWS capture driver commit
- Add amd isp4 driver (cover), commit, commit, commit, commit, commit, commit, commit
- cros-ec: Add Dirkson to the match table commit
- cros-ec: Add Kulnex and Moxoe to the match table commit
- qcom: Add camss support for sm6350 (cover), commit, commit
- qcom: camss: Add camss TPG support for multiple targets (cover), commit, commit, commit
- qcom: camss: Add PIX support for CSID/VFE-340 (cover), commit, commit, commit, commit, commit
- qcom: iris: encoder feature enhancements batch2 (cover), commit, commit, commit, commit, commit, commit
- qcom: iris: add support for decoding 10bit formats (cover), commit, commit, commit, commit, commit, commit
- rc: mceusb: add support for 04eb:e033 commit
- rga: Add RGA3 support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- rockchip: rkcif: add support for rk3588 vicap (cover), commit, commit, commit, commit, commit, commit, commit
- synopsys: enhancements and i.MX95 support (cover), commit, commit, commit, commit, commit, commit
- uvcvideo: Map known XU controls (cover), commit, commit, commit
- Add support for wave6 video codec driver (cover), commit


## 12.9. Serial


## 12.10. Universal Serial Bus

- (FEATURED) Add support for a new protocol over USB4/Thunderbolt cable called USB4STREAM (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- cdns3: USBSSP platform driver support (cover), commit, commit
- cdns3: support configurations without DRD block (cover), commit, commit
- cdnsp: add support for eUSB2v2 port commit
- chipidea: udc: support dynamic gadget add/remove commit, commit
- Add genesys logic gl3590 hub support (cover), commit, commit
- typec: ucsi: Add support for SET_PDOS command (cover), commit, commit, commit
- usb: typec: ucsi: Enable debugfs for message_out data structure commit
- Amd promontory 21 xhci temperature sensor support (cover), commit, commit
- serial: option: add Telit Cinterion FE990D50 compositions commit
- serial: ftdi_sio: add support for E+H FXA291 commit
- serial: option: add TDTECH MT5710-CN commit


## 12.11. Serial Peripheral Interface (SPI)

- Support the spacemit k1 spi controller (cover), commit, commit, commit
- Add qspi support for qcs615 and improve interconnect handling (cover), commit, commit, commit, commit, commit, commit, commit


## 12.12. Watchdog

- Add support for gunyah watchdog (cover), commit, commit
- Add support for andes atcwdt200 (cover), commit, commit, commit
- gpio_wdt: add ACPI support commit
- Add support to read the watchdog bootstatus from imem (cover), commit, commit
- Add reaction control in rti (cover), commit, commit
- Remove driver for integrated wdt of zfx86 486-based soc commit
- Remove amd elan sc520 processor watchdog driver commit


## 12.13. CPU Frequency scaling

- elanfreq: Drop support for AMD Elan SC4* commit
- Add cpufreq scaling support for qualcomm shikra soc (cover), commit, commit


## 12.14. Voltage, current regulators, power capping, power supply

- supply: Add support for Surface RT battery and charger (cover), commit, commit
- Support for samsung s2mu005 pmic and its sub-devices (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- Add support for sgm3804 dual output driver (cover), commit, commit
- mt6359: cleanup and add supplies (cover), commit, commit, commit, commit, commit, commit
- qcom_smd-regulator: Add PM8019 commit
- Add pmau0102 rpmh regulator support (cover), commit, commit
- power: supply: macsmc: Support macOS 27 SMC firmware commit
- Remove used pcap regulator driver commit


## 12.15. Real Time Clock (RTC)

- m41t93: add new features alarm, clock out, watchdog (cover), commit, commit, commit, commit, commit, commit
- rtc: aspeed: add AST2700 compatible commit
- ds1307: add support for clock provider in ds1307 commit
- Remove unused pcap driver commit


## 12.16. Pin Controllers (pinctrl)

- NAdd generic pinctrl for board-level mux chips (cover), commit, commit, commit, commit, commit
- aspeed: Add AST2700 SoC0 support (cover), commit, commit, commit
- aspeed: Add AST2700 SoC1 support (cover), commit, commit, commit
- Add support amlogic a9 (cover), commit, commit
- Add lpass lpi pin controller support for sm6350 (cover), commit, commit, commit, commit, commit
- qcom: Add support for Qualcomm Shikra SoC (cover), commit, commit
- qcom: add support for the TLMM controller on Nord platforms (cover), commit, commit
- Add renesas rz/g3l pincontrol support (cover), commit, commit, commit, commit, commit, commit, commit
- Add tegra238 and tegra264 pinctrl support (cover), commit, commit, commit, commit, commit
- ultrarisc: add DP1000 pinctrl support (cover), commit, commit


## 12.17. Multi Media Card (MMC)

- spacemit: enable SD card support with UHS modes for OrangePi RV2 (cover), commit, commit, commit, commit, commit, commit, commit, commit


## 12.18. Memory Technology Devices (MTD)

- spi-nor: Enhance software protection (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- spi-nor: spansion: fix die erase support (cover), commit, commit
- spinand: Add support for randomizer commit
- spinand: macronix: Enable randomizer support commit
- spinand: Winbond continuous read support (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- maps: remove obsolete impa7 map driver commit


## 12.19. Industrial I/O (iio)

- adc: ad4080: add support for AD4880 dual-channel ADC (cover), commit, commit, commit
- adc: ad4080: add support for AD4884 (cover), commit, commit
- Add support for ad4129-4/8, ad4130-4, and ad4131-4/8 (cover), commit, commit, commit, commit, commit
- adc: ad4691: add driver for AD4691 multichannel SAR ADC family (cover), commit, commit, commit, commit, commit, commit
- adc: qcom-pm8xxx-xoadc: add support for reading channel labels from DT (cover), commit, commit, commit
- Add gpadc support for a523 (cover), commit, commit, commit
- Add support for ad5706r dac (cover), commit, commit
- dac: mcp4821: add configurable gain support (cover), commit, commit, commit
- light: add support for Broadcom APDS9999 (cover), commit, commit
- light: veml3328: add support for new sensor (cover), commit, commit
- magnetometer: add MEMSIC MMC5983MA driver (cover), commit, commit
- Update apds990x als to support device trees (cover), commit, commit, commit


## 12.20. Multi Function Devices (MFD)

- Add MTU3 for RZ/T2H and RZ/N2H (cover), commit, commit, commit, commit
- rsmu: fixes and new IC support (cover), commit, commit
- ezx-pcap: remove unused driver commit


## 12.21. Pulse-Width Modulation (PWM)


## 12.22. Inter-Integrated Circuit (I2C + I3C)


## 12.23. Hardware monitoring (hwmon)

- pmbus: add support for flex bmr316, bmr321, bmr350 and bmr351 commit
- ina238: add samples and update_interval_us support (cover), commit, commit, commit
- temperature: ltc2983: Add support for ADT7604 (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit
- asus-ec-sensors: add rog maximus z790 extreme commit
- Add ROG STRIX B650E-E GAMING WIFI (cover), commit
- Add ROG STRIX B850-E GAMING WIFI commit
- lm63: expose pwm frequency and lut hysteresis as writable commit
- Support active-high alert polarity for lm75 (cover), commit, commit
- nct6683: add support for asrock z890 pro-a commit, (cover)
- Add support for delta e50sn12051 (cover), commit, commit, commit
- pmbus/adm1266: add clear_blackbox, powerup_counter, and rtc debugfs entries (cover), commit, commit, commit
- pmbus/adm1266: blackbox, FW rev, rtc_class, GPIO label (cover), commit, commit, commit, commit
- Add murata d1u74t-w psu driver (cover), commit, commit
- Add support for lx1308 (cover), commit, commit
- Add support for max20830 pmbus (cover), commit, commit
- pmbus/max20860a: add driver for analog devices max20860a (cover), commit, commit
- pmbus/max34440: add support adpm12250 commit
- Add support for infineon efuse xdp730 (cover), commit, commit, commit
- raspberrypi-hwmon voltage support and teardown fix (cover), commit, commit, commit
- Add support for mps mp2985 (cover), commit, commit
- Add driver for arctic fan controller commit
- Support configurable fan pwm at shutdown (cover), commit, commit, commit
- Add support for the ltc4283 hot swap controller (cover), commit, commit, commit
- Add support for microchip emc1812 (cover), commit, commit


## 12.24. General Purpose I/O (gpio)

- gpio-zynq: Add EIO GPIO support (cover), commit, commit, commit
- gpio-f7188x: Add support for NCT6126D version B commit
- 74x164: seed the chain from DT at probe time (cover), commit, commit
- tegra186: Add support for Tegra238 commit
- Add nova lake (nvl) acpi device ids to the usbio gpio and i2c drivers. (cover), commit
- ts5500: remove obsolete driver commit


## 12.25. Leds

- Introduce the multi_max_intensity sysfs attribute (cover), commit
- Add support for ti lp5860 led driver chip commit


## 12.26. DMA engines

- dw-edma: Add Xilinx CPM6-DMA DeviceID commit
- Add peripheral dma support for spacemit k3 soc (cover), commit, commit, commit, commit
- Add support for qcrypto in kaanapali (cover), commit, commit
- sh: rz-dmac: Add DMA ACK signal routing support commit
- Renesas: dmaengine and ASoC fixes (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- Add gpcdma support in tegra264 (cover), commit, commit, commit, commit, commit, commit, commit, commit


## 12.27. Cryptography hardware acceleration

- atmel-ecc: add support for atecc608b commit
- hisilicon/qm: support function reset and VF isolation commit, commit, commit, commit, commit, commit
- qat: add kpt support for gen6 devices commit
- Talitos: fix several issues in the freescale talitos crypto driver (cover), commit, commit, commit, commit, commit, commit, commit, commit, commit, commit, commit
- sun4i-ss - remove insecure and unused rng_alg commit
- xilinx-trng: remove crypto_rng interface commit
- crypto4xx: remove insecure and unused rng_alg commit
- exynos-rng: remove exynos-rng driver commit
- hisi-trng: move hisi-trng into drivers/char/hw_random/ commit


## 12.28. PCI

- endpoint: pci-ep-msi: Add embedded doorbell fallback (cover), commit, commit, commit, commit, commit, commit, commit
- qcom: Add D3cold support (cover), commit, commit, commit
- PCI/switchtec: Add Gen6 Device IDs commit
- Add pcie support for ultrarisc dp1000 soc (cover), commit, commit


## 12.29. Clock

- canaan: Add support for K230 clock (cover), commit, commit
- qcom: Add initial clock controllers for the upcoming Hawi SoC (cover), commit, commit, commit, commit, commit, commit, commit
- qcom: milos: Add GX clock controller (cover), commit, commit
- Add support for videocc and camcc on x1p42100 platform (cover), commit, commit, commit, commit, commit
- Add cmn pll clock controller support for ipq5332 (cover), commit, commit, commit
- Support enabling interconnect path for gdsc for fixing milos camcc (cover), commit, commit, commit, commit
- Add support for DU and DSI on the Renesas RZ/G3E SoC (cover), commit, commit, commit, commit, commit, commit
- renesas: r9a09g047: Add support for SMUX2_DSI{0,1}_CLK commit


## 12.30. PHY ("physical layer" framework)

- Add driver for eyeq5 ethernet phy wrapper commit
- Add basic support for NXPs TJA1145 CAN transceiver] (cover), commit
- Add emmc phy support for axiado ax3000 soc (cover), commit, commit, commit
- econet: Add PCIe PHY driver for EcoNet SoCs (cover), commit, commit
- spacemit: Add USB2 PHY support for K3 SoC (cover), commit, commit
- lynx-28g: add support for big endian register maps (cover), commit, commit, commit, commit, commit, commit
- lynx-28g: add support for 25GBASER (cover), commit, commit, commit, commit, commit, commit, commit
- phy: ti: add PHY driver for TI DS125DF111 Dual-Channel Retimer commit
- J722s sgmii support (cover), commit, commit, commit, commit, commit


## 12.31. EDAC (Error Detection And Correction)

- igen6: Add Intel Nova Lake-H SoC support commit
- igen6: Add one Intel Panther Lake-H SoC support commit
- Add RRL support for Intel Diamond Rapids servers commit, commit, commit, commit, commit, commit, commit, commit


## 12.32. Various

- accel/amdxdna
- accel/ivpu: Add support for limiting NPU frequency commit
- mhi: host: pci_generic: Add Telit FE910C04 modem support commit
- Rework scmi transport drivers probing sequence (cover), * Hot-join improvements and mipi hci hot-join support (cover), commit, commit, commit, commit, commit, commit, commit, commit
- nfc: nxp-nci: Add ISO15693 support commit
- 8250_mtk: Add ACPI support commit
- synclink_gt: remove broken driver commit
- applicom: remove low-quality, unused driver commit
- dtlk: remove driver for ISA speech synthesizer card commit
- imu: st_lsm6dsx: Add support for rotation sensor (cover), commit, commit, commit, commit, commit, commit
- mailbox: Add list of used channels to debugfs commit


# 13. List of Pull Requests


# 14. Other news sites

- LWN's merge window part 1, part 2
- Development statistics for the 7.2 kernel
- Phoronix's Feature overview
