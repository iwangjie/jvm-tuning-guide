### 1. GitHub 仓库目录结构 (Repository Structure)

采用 `docs/` 存放核心文档，`examples/` 存放配置示例，`versions/` 存放特定版本的详细说明，这样结构清晰，便于维护。

```
jvm-tuning-mastery/
│
├── docs/                       # 核心文档目录
│   ├── 01-basics/              # 基础知识与准备
│   │   ├── 01-why-tuning.md
│   │   ├── 02-monitoring-tools.md
│   │   ├── 03-parameter-syntax.md
│   │
│   ├── 02-memory-management/   # 内存管理
│   │   ├── 01-heap-structure.md
│   │   ├── 02-heap-sizing.md           # -Xms, -Xmx, -Xmn
│   │   ├── 03-metaspace.md             # 元空间/永久代
│   │   ├── 04-direct-memory.md
│   │
│   ├── 03-garbage-collection/  # 垃圾回收 (GC)
│   │   ├── 01-gc-fundamentals.md
│   │   ├── 02-gc-logging.md            # GC日志解读
│   │   ├── 03-collectors-overview.md
│   │   ├── 04-tuning-g1.md
│   │   ├── 05-tuning-zgc-shenandoah.md
│   │
│   ├── 04-jit-and-execution/   # JIT编译器与执行
│   │   ├── 01-c1-c2-graal.md
│   │   ├── 02-code-cache.md
│   │
│   ├── 05-troubleshooting/     # 故障排查与分析
│       ├── 01-analyzing-oom.md
│       ├── 02-thread-dumps.md
│
├── examples/                   # 开箱即用的参数示例
│   ├── low-latency-g1.params
│   ├── high-throughput-parallel.params
│   ├── large-heap-zgc.params
│   ├── microservice-defaults.params
│
├── versions/                   # 特定Java版本的调优差异
│   ├── java8-tuning.md         # Java 8 (CMS的最后荣光与G1初探)
│   ├── java11-tuning.md        # Java 11 (G1成为默认，ZGC实验性引入)
│   ├── java17-tuning.md        # Java 17 (ZGC/Shenandoah生产可用)
│   ├── java21-tuning.md        # Java 21 (分代ZGC，虚拟线程影响)
│
├── CONTRIBUTING.md             # 贡献指南
├── LICENSE
└── README.md                   # 仓库入口和总览（包含下方的大纲链接）
``
