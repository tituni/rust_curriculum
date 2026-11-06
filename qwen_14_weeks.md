# Safe Software Development with Rust

**Format:** One-semester course (15 weeks), 6 ECTS — 2 h lecture + 3 h lab per week
**Prerequisites:** C/C++ programming (incl. pointers, classes, templates), basic data structures, basic OS concepts
**Assumption:** no prior Rust knowledge required; C++ knowledge is actively leveraged as the contrast baseline

---

## Learning Outcomes

After completing the course, students can:

1. Explain why undefined behavior and memory unsafety are systemic hazards, and contrast C/C++ semantics with Rust's guarantees
2. Reason about ownership, borrowing, and lifetimes; diagnose and resolve borrow-checker errors
3. Write idiomatic, zero-cost Rust using traits, generics, pattern matching, and `Result`-based error handling
4. Write and audit `unsafe` code, encapsulating it behind documented, invariant-checked safe APIs
5. Build data-race-free concurrent and asynchronous programs
6. Integrate Rust with C/C++ via FFI and design the migration of a legacy component
7. Work in `no_std`/embedded environments and place Rust in the context of safety standards (ISO 26262, MISRA, CERT)
8. Apply safety toolchains: Clippy, Miri, fuzzing, property-based testing, and lightweight verification (Kani)
9. Produce a safety-oriented design report with a small "safety case" for a `unsafe` surface

---

## Weekly Plan

| Wk | Topic | Lab / Exercises |
|----|-------|-----------------|
| 1 | **Why safety, why Rust.** Cost of memory unsafety (CVEs, "The Cost of Memory Safety"), C/C++ UB tour; toolchain (rustup, cargo, clippy, rustfmt); C++→Rust mapping overview (RAII→Drop, smart pointers→Box/Arc/Rc, exceptions→Result, templates→generics) | Toolchain setup; port a small C++ program to Rust |
| 2 | **Data & control flow.** Integers, `Option`, `Result` (preview), enums with data vs C++ tagged unions, pattern matching, exhaustiveness | Write an arithmetic expression interpreter with proper error variants |
| 3 | **Ownership & moves.** Value/move semantics vs C++ copy/move, `Drop` and RAII contrast, `Clone`/`Copy` | Implement a small object pool with manual lifetime reasoning |
| 4 | **Borrowing & lifetimes.** References, mutability rules, lifetime annotations, common error patterns; comparison to C++ reference/dangling scenarios | "Fighting the borrow checker" problem set (dedicated extra lab time) |
| 5 | **Structs, traits, generics.** Trait objects vs dynamic dispatch, monomorphization vs C++ template instantiation, code-size trade-offs | Design a container library: stack/queue/HashMap wrappers behind traits |
| 6 | **Error handling.** `Result`, the `?` operator, panics vs exceptions, `std::error::Error`, `thiserror`/`anywhere`, error taxonomy design | Library with layered custom errors; compare with C++ exception design |
| 7 | **Zero-cost abstractions.** Iterators, closures and move semantics, collections; `for`-loops desugared; iterator combinators vs C++ ranges | Reimplement a C++ algorithm file (filter/map/reduce pipeline) in idiomatic Rust |
| 8 | **Midterm** (quiz on W1–7 + first project checkpoint) — then **`unsafe` I**: what `unsafe` unlocks, sources of UB, UB examples from real C++ code, Miri | Introduce Miri; find a UB bug in provided C++ snippet, fix in Rust |
| 9 | **`unsafe` II.** Invariants, documenting invariants, building safe abstractions over `unsafe`, auditing an `unsafe` surface, `unsafe`-free vs `-restricted` design | Audit a provided ~500-line `unsafe` crate; write invariant docs |
| 10 | **Concurrency I.** Threads, `Send`/`Sync` and what they encode, `Arc`/`Mutex` vs C++ `shared_ptr`/`mutex`, data-race freedom by design | Thread-safe LRU cache with benchmarks vs C++ reference |
| 11 | **Concurrency II.** Channels (`mpsc`), message-passing design, scoped threads, async/await, futures, event loops | Build a small async task scheduler; port a C++ callback design to async |
| 12 | **FFI & C/C++ interop.** `extern "C"`, `repr(C)`, C ABI, calling C from Rust, exposing Rust to C/C++ (cbindgen, `cxx`), memory-layout pitfalls, strategy for migrating a legacy component | Replace one function of a provided C++ module with Rust, keep C++ build green |
| 13 | **`no_std` & embedded + safety landscape.** `no_std`, alloc, interrupts, determinism; where Rust stands w.r.t. ISO 26262, MISRA, CERT; tooling status and open research | Bare-metal GPIO/UART program (simulator or RISC-V/HAL) |
| 14 | **Testing & verification at scale.** Unit/integration tests, fuzzing (cargo-fuzz), property-based testing (proptest), Miri, Kani for simple proofs, clippy lint discipline; writing a mini safety case | Fuzz the W2 parser; Kani-verify an arithmetic module invariant |
| 15 | **Capstone presentations** (incl. peer code review), course wrap-up, Rust in industry (Linux kernel, Android, cloud infra, automotive research), career paths | Present capstone + safety report |

---

## Projects

**Continuous mini-labs** (every week, auto-graded): build fluency and idiom.

**Midterm project (W5–8, ~20%):** a library component with documented invariants, full test suite, no `unsafe` (or a justified, bounded `unsafe` surface). Examples: memory pool, safe config-parser, thread-safe key-value store.

**Capstone (W9–15, ~40%)** — choose one track:

1. **Legacy migration:** reimplement a meaningful C/C++ component in Rust, interoperate via FFI, and document the safety delta (found UB, reduced attack surface)
2. **Embedded/no_std:** device driver or RTOS task with deterministic timing, validated on simulator or hardware
3. **Protocol safety:** implement a wire-format parser (e.g., HTTP subset, CAN frames) hardened with fuzzing + property tests; include a "safety case" report
4. **Safety audit:** analyze a real open-source C++ codebase, quantify UB/unsafety risk, and present a Rust redesign with API sketch

Each capstone submission includes: code, clippy-clean, tests + fuzz results, and a 5–10 page report with an explicit `unsafe` audit section.

---

## Assessment

| Component | Weight |
|-----------|--------|
| Weekly labs + quizzes | 20% |
| Midterm (quiz + project) | 20% |
| Capstone (code + safety report + presentation) | 40% |
| Peer code review / participation | 10% |

*Optional final written exam available for credit-capped registration.*

---

## Core Reading & References

- *The Rust Programming Language* (2nd ed.) — primary text
- *Rust by Example*, *The Rustonomicon* (unsafe), *Rust API Guidelines*
- *Programming: Principles and Practice*-style contrast material: C++20/23 standard library headers for side-by-side comparison
- **Standards/context:** CERT C Secure Coding, MISRA C/C++ guidelines, ISO 26262 overview, Rust Safety Council documentation
- **Tooling docs:** Clippy, Miri, cargo-fuzz, proptest, Kani, cxx/cbindgen
- Selected papers/talks: *The Cost of Memory Safety* (2020), CppCon "fearless concurrency" talks, Rust in the Linux Kernel docs

---

## Teaching Notes for a C++-Experienced Cohort

- **Lean into contrasts:** every concept should be anchored to the C++ equivalent (RAII→`Drop`, `unique_ptr`→`Box`, `shared_ptr`→`Arc`, exceptions→`Result`, templates→generics+monomorphization, dangling references→borrow checker). Provide a living C++→Rust cheat sheet in week 1.
- **Expect the borrow-checker wall in weeks 3–4:** budget extra lab time and office hours; it's the single biggest churn point for C++ students.
- **Keep `unsafe` honest:** the course's differentiator vs. a "Rust 101" is the W8–10 and W13–14 material — invariants, auditing, standards context, verification tooling.
- Pin a stable toolchain (e.g., edition 2021/2024) at course start to avoid version drift.

---

Want me to adjust anything — e.g., a 20-week term, heavier embedded or verification emphasis, 4 ECTS instead of 6 — or generate the week-1 lecture outline and the C++→Rust cheat sheet as a first step?