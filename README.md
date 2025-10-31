# Iridium77 🪐

**Iridium77** is a mid-level Rust project that guides you through building a **lightweight, crash-safe key–value database** from scratch.  
It’s inspired by RocksDB and Bitcask but focuses on **clarity, correctness, and concurrency** — perfect for intermediate Rust developers expanding into systems programming.

---

## 🧭 Project Purpose

This project teaches how databases work **beneath the ORM** — from file I/O and serialization to async concurrency and graceful shutdowns.  
You’ll build each layer step by step, learning _why_ every component exists.

---

## 🎓 Skill Level

**Intermediate Rust developers** who:

- Understand ownership, lifetimes, traits, and `Result` handling.
- Have used `tokio` or async code before.
- Want to explore systems programming and database internals.

---

## 🏗️ Architecture Overview

Iridium77 follows a simplified **LSM (Log-Structured Merge) design**:

1. **Write-Ahead Log (WAL)** – ensures durability by logging all changes first.
2. **Memtable** – a sorted in-memory store for recent writes (`BTreeMap`).
3. **SSTables** – immutable, sorted on-disk tables flushed from memory.
4. **Compaction (basic)** – merges old SSTables occasionally to reclaim space.
5. **CLI Interface** – easy way to test and inspect your database.

You’ll also add:

- **Concurrency safety** via `Arc` and `RwLock`.
- **Tracing & logging** with the `tracing` crate.
- **Benchmarks** with `criterion`.

---

## 🧩 Core API

```rust
pub trait Storage {
    fn put(&self, key: Vec<u8>, value: Vec<u8>) -> Result<()>;
    fn get(&self, key: &[u8]) -> Result<Option<Vec<u8>>>;
    fn delete(&self, key: &[u8]) -> Result<()>;
}
```

Extensions (optional):

- Range scans
- Async I/O (tokio::fs)
- Basic networking (client/server mode)

| Area              | Crate                           |
| ----------------- | ------------------------------- |
| Async runtime     | `tokio`                         |
| Serialization     | `serde`, `bincode`              |
| Logging & tracing | `tracing`, `tracing-subscriber` |
| CLI               | `clap`                          |
| Testing           | `proptest`, `tempfile`          |
| Benchmarking      | `criterion`                     |
| Error handling    | `thiserror`, `anyhow`           |

# Project Layout

```
/iridium77             # core library
  /src
    lib.rs
    wal.rs
    memtable.rs
    sstable.rs
    db.rs
/cli                   # CLI interface
  src/main.rs
/docs                  # documentation
```

🕒 Suggested Development Roadmap
🧩 Week 1 – Setup & In-Memory DB
Initialize a Cargo workspace (iridium77, cli).
Create the Storage trait and MemStore implementation.
Add a simple CLI for put/get/delete.
✅ Goal: Fully working in-memory key–value store.

💾 Week 2 – Write-Ahead Log (WAL)
Create an append-only log file (wal.log).
Write put and delete operations to disk.
Implement recovery on startup (replay log).
✅ Goal: Crash-safe persistence.

📦 Week 3 – SSTables & Flush
Serialize sorted data to disk when the memtable is full.
Use a simple file format: header → key-value pairs → footer.
On startup, load SSTables for reads.
✅ Goal: Data persists across restarts without replaying the log every time.

⚙️ Week 4 – Compaction (Basic)
Merge multiple SSTable files into one.
Remove deleted keys (tombstones).
Replace old files with the compacted one.
✅ Goal: Maintain fast lookups and small disk usage.

🔎 Week 5 – Logging, Metrics & CLI Polish
Add logging for key events (put, flush, compact).
Add a stats command showing file counts, memtable size, etc.
Include --verbose flag to toggle tracing output.
✅ Goal: Make the database observable and developer-friendly.

🌐 Optional (Advanced)
If you finish early, try one of these:
Add a simple TCP server for remote access (tokio::net).
Expose a Prometheus metrics endpoint.
Add async file I/O for flush/compaction.
