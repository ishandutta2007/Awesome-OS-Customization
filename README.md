# Awesome-OS-Customization

## Top Out-of-Core DataFrames Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Larger-Than-RAM Processing, Lazy Evaluation & Memory-Mapped Analytics*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Out-of-Core DataFrames**. These tools let you analyze datasets that exceed available RAM — using memory mapping, lazy evaluation, streaming engines, and disk spilling to process billions of rows on a single machine or across a cluster without loading everything into memory at once.

**Examples** include Polars, DuckDB, Vaex, Dask, Modin, PySpark, Ray Data, FireDucks, and Ibis (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, lazy streaming, and transparent memory behavior — ideal for data engineers hitting pandas' memory ceiling, analysts processing large Parquet files on a laptop, and teams deciding between single-machine out-of-core and distributed cluster approaches.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Databricks** — Managed Apache Spark platform with Delta Lake, Photon engine, and serverless compute. The enterprise default for distributed out-of-core data processing at scale.

- **Snowflake** — Cloud data warehouse with automatic query optimization and elastic compute. Handles out-of-core processing server-side without client-side memory concerns.

- **MotherDuck** — Managed DuckDB cloud service with hybrid execution that queries local and cloud data seamlessly.

## Open-Source GitHub Projects

- **[Polars](https://github.com/pola-rs/polars)**  
  **The Rust-native DataFrame with a streaming engine for larger-than-RAM workloads** — MIT licensed, **30,000+ GitHub stars** . **Lazy evaluation, SIMD, multi-threaded execution, and a dedicated streaming engine** that processes data in chunks rather than loading everything into memory . **The new streaming engine** (from v1.31.1) is designed to become the default, usually faster and using less memory than the in-memory engine, with `LazyFrame.collect(engine='streaming')` or `LazyFrame.sink_*(...)` for streaming results larger than RAM to disk . **All queries that run on the in-memory engine should run on the streaming engine**, with some operations transparently falling back to in-memory if a native streaming implementation isn't ready yet . **Best for single-machine production ETL** where you want pandas-like ergonomics with Rust performance and a streaming escape hatch when data exceeds RAM .

- **[DuckDB](https://github.com/duckdb/duckdb)**  
  **The embedded OLAP engine with automatic spill to disk** — MIT licensed . **SQL-first analytics on CSV and Parquet files without loading them into memory first** . **Automatic out-of-core execution**: when a query's working set exceeds `memory_limit`, DuckDB spills to disk rather than failing . **Apache Arrow integration** enables zero-copy data exchange with Polars, pandas, and other Arrow-aware tools . **The simplest path to out-of-core analytics** — no cluster, no distributed scheduler, just a single process that handles larger-than-RAM data gracefully . **Best for SQL-first workflows** and querying files directly where you want minimal infrastructure .

- **[Vaex](https://github.com/vaexio/vaex)**  
  **The memory-mapped, lazy out-of-core DataFrame for exploratory analytics** — MIT licensed, **8,500+ GitHub stars** . **Instant opening of huge HDF5 and Apache Arrow files via memory mapping** — no data is loaded into memory until needed . **Lazy streaming from S3 supported in combination with memory mapping** . **Expression system** that lazily transforms data on demand without wasting memory on feature engineering . **Filtering and evaluating expressions do not waste memory by making copies** — data stays untouched on disk and streams only when needed, delaying the time before you need a cluster . **Fast groupby/aggregations** especially with categories, exceeding **one billion rows per second** in favorable cases . **Joins don't copy the right table**, saving gigabytes of memory with sub-second joining on a billion rows . **Visualization via histograms, density plots, and 3D volume rendering** for interactive exploration of big data . **Best for exploratory analytics** on large tabular datasets where you want to visualize and drill down without a cluster .

- **[Dask](https://github.com/dask/dask)**  
  **The distributed task-graph engine that mimics pandas/NumPy for big data** — BSD-3-Clause licensed . **Scales pandas DataFrames across multiple cores or machines** via partitioned data and lazy task graphs . **The canonical choice when your bottleneck is memory limits** — out-of-core processing for larger-than-RAM datasets . **Multiple schedulers**: threads (lowest overhead ~10 µs/task, best for numeric work), processes (~10 ms/task, bypasses the GIL for Python-heavy work), synchronous (debugging), and distributed (dashboard, multi-machine clusters) . **Critical performance rule**: **don't load data locally then hand to Dask** — let Dask handle loading via `dd.read_csv()` or `dd.read_parquet()` rather than `pd.read_csv()` followed by `dd.from_pandas()` . **Avoid repeated `compute()` calls** — each is a separate execution . **Best for teams needing to scale existing pandas workflows** across cores or a cluster without rewriting logic in a new framework .

- **[Modin](https://github.com/modin-project/modin)**  
  **The drop-in pandas replacement that scales on Ray or Dask clusters** — Apache-2.0 licensed . **Change one import line** (`import modin.pandas as pd`) and use all your CPU cores . **By default, Modin leverages out-of-core methods to handle datasets that don't fit in memory for both Ray and Dask engines** . **Best for users wanting parallelism and out-of-core with zero code changes** — the lowest-friction migration path from pandas, though with the caveat that "out-of-the-box" claims may require cluster configuration for true larger-than-RAM datasets .

- **[PySpark](https://github.com/apache/spark)**  
  **The industry standard for distributed computing across machines** — Apache-2.0 licensed . **The enterprise cluster scale answer** when a single machine's out-of-core processing is no longer enough . **Spark SQL, DataFrames, and MLlib** for distributed analytics at petabyte scale . **Best for enterprise cluster deployments** where data volume and organizational infrastructure justify distributed compute .

- **[Ray Data](https://github.com/ray-project/ray)**  
  **Distributed data processing built on Ray** — Apache-2.0 licensed . **Streaming execution for ML workloads** with native integration to Ray Train and Ray Tune . **Best for ML pipelines** needing distributed data loading and preprocessing alongside model training .

- **[FireDucks](https://github.com/fireducks-dev/fireducks)**  
  **The JIT-compiled drop-in pandas accelerator** — **achieves up to 20.77x speedup over pandas** on sorting a 10-million-row dataset, with an **average 7.28x speedup across seven common workloads** . **Uses lazy execution, compiler optimization, and multithreaded CPU processing** behind the scenes while maintaining pandas API compatibility . **Benchmark results**: Parquet read 2.66x, filter several columns 11.63x, low-cardinality groupby 15.44x, high-cardinality groupby 5.75x, sort 20.77x, join with 2M-row lookup 3.20x . **A research paper demonstrates up to 86.6x speedup on finance-inspired workloads** via define-by-run IR construction and whole-program DataFrame optimizations . **Limitation**: **not a complete drop-in replacement** — FireDucks DataFrames are different objects internally, and some pandas features or third-party libraries may differ . **Best for pandas users wanting significant speedups with zero syntax changes** on workloads that fit in memory or can benefit from lazy optimization .

- **[Ibis](https://github.com/ibis-project/ibis)**  
  **The unified Python API across DuckDB, BigQuery, Snowflake, and more** — Apache-2.0 licensed . **Write DataFrame code once, execute on any backend** — useful for teams wanting backend portability without rewriting analytics logic . **Best for multi-backend workflows** where the same analysis must run against different engines .

### Additional Strong Open-Source Options

- **pandas** — The reference DataFrame library. Not out-of-core by default, but the baseline all other tools are compared against and often migrated from .
- **Polars streaming engine** — Polars' dedicated larger-than-RAM engine, usable via `collect(engine='streaming')` or `sink_parquet()` for streaming results to disk .
- **Dask Bag** — Unstructured data processing (JSON, logs, text) with streaming and memory-efficient operations, convertible to DataFrame for structured analysis .
- **cuDF (RAPIDS)** — GPU-accelerated DataFrame library that mimics pandas, with Dask integration for scaling across multiple GPUs .

**Frameworks for building custom systems**: Combine **Polars** for single-machine production ETL with a streaming engine for larger-than-RAM data, **DuckDB** for SQL-first analytics with automatic spill to disk and zero-copy Arrow exchange, **Vaex** for memory-mapped exploratory analytics on billion-row datasets without a cluster, **Dask** for scaling existing pandas workflows across cores or machines, and **PySpark** when data volume genuinely requires distributed compute — to build an out-of-core stack tailored to your workload size, infrastructure, and tolerance for cluster complexity.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Out-of-core tools differ significantly in execution model, infrastructure requirements, and migration friction. **The right choice depends on your bottleneck** — speed on a single machine favors Polars or FireDucks, memory limits favor Dask or Vaex, SQL-first workflows favor DuckDB, and enterprise cluster scale favors PySpark . **DuckDB's automatic spill to disk** makes it the simplest path to out-of-core SQL, but **sandboxing untrusted SQL and out-of-core execution can conflict** — disabling filesystem access also disables spilling unless the temp directory is explicitly allowlisted . **Vaex's memory mapping** is excellent for exploratory analytics but less suited for complex joins or writes. **Dask requires letting it handle data loading** — loading locally then handing to Dask defeats the purpose and can cause OOM . **FireDucks is not a complete drop-in replacement** despite its compatibility claims — some pandas features and third-party libraries may differ . **Modin's out-of-core claims may require cluster configuration** for true larger-than-RAM datasets.
- Self-hosted open-source solutions require proper memory limit configuration, disk space for spilling, and awareness that "out-of-core" often means "slower but possible" rather than "as fast as in-memory."

---

**Made for data engineers, analysts, and teams processing datasets larger than memory.**
Let's make out-of-core data processing more open, memory-efficient, and approachable.
