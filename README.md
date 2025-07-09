## JVM 调优精通指南 (JVM Tuning Mastery Guide)

本项目旨在提供一份全面、循序渐进的 JVM 参数调优指南，涵盖从基础概念到高级实践，并针对 Java 主流版本（8, 11, 17, 21+）提供差异化指导和开箱即用的参数配置。

### 📚 目录大纲

#### 前言
*   [项目目标与适用人群](README.md)
*   [如何使用本指南](README.md)

#### 第 1 章：基础入门与准备工作 (docs/01-basics/)
本章介绍 JVM 调优的必要性、常用工具以及参数的基础知识。

*   **1.1 为什么要进行 JVM 调优？** (01-why-tuning.md)
    *   性能指标：吞吐量 (Throughput)、延迟 (Latency)、内存占用 (Footprint)。
    *   调优的误区：不要过度调优，不要盲目复制参数。
*   **1.2 监控与诊断工具** (02-monitoring-tools.md)
    *   命令行工具：`jps`, `jstat`, `jinfo`, `jmap`, `jstack`。
    *   可视化工具：JVisualVM, JConsole, Arthas。
    *   高级工具：JFR (Java Flight Recorder) 和 JMC (Java Mission Control)。
*   **1.3 JVM 参数分类与语法** (03-parameter-syntax.md)
    *   标准参数 (`-`)。
    *   非标准参数 (`-X`)。
    *   不稳定参数 (`-XX`)：布尔型 (`-XX:+/-Flag`) 和键值型 (`-XX:Flag=value`)。

#### 第 2 章：内存管理与参数 (docs/02-memory-management/)
JVM 调优的第一步通常是合理的内存分配。

*   **2.1 JVM 内存结构概览** (01-heap-structure.md)
    *   堆 (Heap)：年轻代 (Eden, Survivor) 与老年代 (Old Gen)。
    *   非堆 (Non-Heap)：元空间 (Metaspace)、线程栈 (Stack)、直接内存 (Direct Memory)。
*   **2.2 堆内存调优** (02-heap-sizing.md)
    *   设置堆大小：`-Xms`, `-Xmx` (为什么建议设置为相同值？)。
    *   设置年轻代：`-Xmn`, `-XX:NewRatio`, `-XX:SurvivorRatio`。
    *   对象晋升阈值：`-XX:MaxTenuringThreshold`。
*   **2.3 元空间（及永久代）调优** (03-metaspace.md)
    *   Java 8 前后的区别 (PermGen vs Metaspace)。
    *   关键参数：`-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize`。
*   **2.4 堆外内存与直接内存** (04-direct-memory.md)
    *   `-XX:MaxDirectMemorySize` 及其监控。

#### 第 3 章：垃圾回收 (GC) 调优 (docs/03-garbage-collection/)
GC 是 JVM 调优的核心，也是最复杂的部分。

*   **3.1 GC 基础概念** (01-gc-fundamentals.md)
    *   Minor GC, Major GC, Full GC 的区别。
    *   STW (Stop-The-World) 现象。
    *   并发标记、并发清理等术语解释。
*   **3.2 GC 日志分析（重要！）** (02-gc-logging.md)
    *   启用 GC 日志参数 (Java 8 vs Java 9+ 的统一日志框架 `-Xlog:gc*`)。
    *   如何阅读和分析 GC 日志（结合 GCEasy 等工具）。
*   **3.3 垃圾收集器概览** (03-collectors-overview.md)
    *   Serial, Parallel (吞吐量优先)。
    *   CMS (低延迟的里程碑，已废弃)。
    *   G1 (Garbage-First，区域化收集)。
    *   ZGC & Shenandoah (下一代超低延迟收集器)。
*   **3.4 G1 收集器调优** (04-tuning-g1.md)
    *   G1 的工作原理 (Region, Mixed GC)。
    *   核心参数：`-XX:MaxGCPauseMillis` (设置目标停顿时间)。
    *   `-XX:G1HeapRegionSize`, `-XX:InitiatingHeapOccupancyPercent` (IHOP)。
*   **3.5 ZGC 与 Shenandoah 调优** (05-tuning-zgc-shenandoah.md)
    *   适用场景与优势（TB级大堆，超低延迟）。
    *   核心参数（通常很少，主要依赖 `-Xmx`）。
    *   分代 ZGC (Java 21+)。

#### 第 4 章：JIT 编译器与执行优化 (docs/04-jit-and-execution/)
了解代码是如何被编译和执行的。

*   **4.1 JIT 编译器：C1, C2 与 Graal** (01-c1-c2-graal.md)
    *   解释执行与编译执行。
    *   分层编译 (`-XX:+TieredCompilation`)。
*   **4.2 代码缓存 (Code Cache)** (02-code-cache.md)
    *   存放 JIT 编译后的本地代码。
    *   `-XX:InitialCodeCacheSize`, `-XX:ReservedCodeCacheSize`。

#### 第 5 章：故障排查与案例分析 (docs/05-troubleshooting/)
解决实际生产中遇到的问题。

*   **5.1 内存溢出 (OOM) 分析** (01-analyzing-oom.md)
    *   Heap OOM, Metaspace OOM, Direct Memory OOM。
    *   自动导出 Dump 文件：`-XX:+HeapDumpOnOutOfMemoryError`, `-XX:HeapDumpPath`。
    *   使用 MAT 或 JVisualVM 分析 Dump 文件。
*   **5.2 线程分析与死锁排查** (02-thread-dumps.md)
    *   使用 `jstack` 生成线程快照。
    *   分析线程状态 (RUNNABLE, BLOCKED, WAITING)。

#### 第 6 章：版本特性与迁移指南 (versions/)
区分主流 Java 版本的默认行为和特有参数。

*   **6.1 Java 8 调优要点** (java8-tuning.md)
    *   默认 Parallel GC。
    *   CMS 的使用与注意事项。
    *   永久代到元空间的过渡。
*   **6.2 Java 11 调优要点** (java11-tuning.md)
    *   G1 成为默认收集器。
    *   ZGC 实验性引入。
    *   字符串去重参数 (`-XX:+UseStringDeduplication`)。
*   **6.3 Java 17 调优要点** (java17-tuning.md)
    *   ZGC 和 Shenandoah 生产可用。
    *   移除 CMS 收集器。
*   **6.4 Java 21+ 调优要点** (java21-tuning.md)
    *   分代 ZGC (`-XX:+ZGenerational`)。
    *   虚拟线程 (Virtual Threads) 对 JVM 调优的影响。

#### 第 7 章：开箱即用的参数模板 (examples/)
针对不同应用场景，提供可以直接使用的参数组合（请根据实际情况微调）。

*   **7.1 高吞吐量批处理应用** (high-throughput-parallel.params)
    *   推荐收集器：Parallel GC。
    *   参数示例与说明。
*   **7.2 低延迟 Web API 服务 (中小型堆)** (low-latency-g1.params)
    *   推荐收集器：G1 GC。
    *   参数示例与说明。
*   **7.3 超大堆内存应用 (如搜索、缓存)** (large-heap-zgc.params)
    *   推荐收集器：ZGC。
    *   参数示例与说明。
*   **7.4 微服务/容器环境默认配置** (microservice-defaults.params)
    *   容器感知参数 (`-XX:+UseContainerSupport`)。
    *   内存自适应调整。
