Done — embedded/`no_std` removed, scope is now general-purpose programming (services, libraries, systems software, C/C++ interop). Standards context reoriented from automotive/embedded (ISO 26262, MISRA) toward general-purpose security (CERT, CWE, supply chain).

# Safe Software Development with Rust *(Intensive, 7-week module — general purpose)*

**Format:** 7 weeks, 5 ECTS (≈ 130 h total workload)
**Prerequisites:** C/C++ programming (incl. pointers, classes, templates), basic data structures, basic OS concepts
**Assumption:** no prior Rust required; C++ knowledge is actively used as the contrast baseline

## Workload breakdown (5 ECTS ≈ 130 h)

| Component | Hours |
|-----------|-------|
| Contact: 3 h lecture + 5 h lab × 7 wks | 56 h |
| Self-study (reading, exercises, projects, capstone + report) | ~74 h |
| **Total** | **~130 h** |

---

## Learning Outcomes

Students can:

1. Explain why memory unsafety/UB is a systemic hazard; contrast C/C++ semantics with Rust's guarantees
2. Reason about ownership, borrowing, and lifetimes; diagnose and resolve borrow-checker errors
3. Write idiomatic, zero-cost Rust using traits, generics, pattern matching, and `Result`-based errors
4. Write and audit `unsafe` code behind documented, invariant-checked safe APIs
5. Build data-race-free concurrent and asynchronous network services
6. Integrate Rust with C/C++ via FFI and plan a legacy-component migration
7. Apply security context: map Rust's guarantees to CWE/CERT classes, manage dependencies and supply-chain risk
8. Apply the safety toolchain: Clippy, Miri, fuzzing, property-based testing, lightweight verification (Kani)
9. Produce a security/safety-oriented design report with a small "safety case" for an `unsafe` surface

---

## Weekly Plan *(dense — each week carries lecture + lab)*

| Wk | Topic | Lab / Exercises |
|----|-------|-----------------|
| 1 | **Foundations: why safety, why Rust + ownership.** Cost of memory unsafety (CVEs, *The Cost of Memory Safety*), C/C++ UB tour, toolchain (rustup, cargo, clippy, rustfmt); C++→Rust map (RAII→`Drop`, `unique_ptr`→`Box`, `shared_ptr`→`Arc`, exceptions→`Result`, templates→generics); ownership, moves, `Drop` | Toolchain setup; port a small C++ program; small object pool with manual lifetime reasoning |
| 2 | **Data, types, traits & error handling.** `Option`/`Result`, enums-with-data vs C++ tagged unions, pattern matching, exhaustiveness; structs, traits, trait objects vs C++ dynamic dispatch, monomorphization; `Result`, `?`, panics vs exceptions, `thiserror` | Expression interpreter with proper error variants; container library behind traits; layered custom errors |
| 3 | **Borrowing, lifetimes & zero-cost abstractions.** References, mutability rules, lifetime annotations, common error patterns (dangling-reference contrast); iterators, closures, collections, `for`-desugaring | **"Fighting the borrow checker" set** (extra lab slot); port a C++ algorithm (filter/map/reduce) pipeline |
| 4 | **`unsafe` I & II: invariants, abstractions, auditing.** What `unsafe` unlocks, sources of UB, real C++ UB examples, Miri; invariants, documenting, building safe APIs over `unsafe`, auditing an `unsafe` surface, unsafe-restricted design | Miri: find a UB bug in provided C++ and fix in Rust; audit a ~500-line `unsafe` crate; write invariant docs |
| 5 | **Concurrency & async.** Threads, `Send`/`Sync` (what they encode), `Arc`/`Mutex` vs C++, data-race freedom by design; channels (`mpsc`), scoped threads, async/await, futures, event loops | Thread-safe LRU cache (benchmarked vs C++); small async task scheduler; port a C++ callback design to async |
| 6 | **FFI, C/C++ interop & security landscape.** `extern "C"`, `repr(C)`, C ABI, calling C from Rust, exposing Rust to C/C++ (cbindgen, `cxx`), memory-layout pitfalls, migration strategy; security context: Rust guarantees vs CWE Top 25 / CERT, dependency & supply-chain hygiene (cargo audit, vendoring), reviewing `unsafe` in dependencies | Replace one function of a provided C++ module in Rust keeping the C++ build green; dependency-audit + security review of a real Rust crate |
| 7 | **Testing, verification & capstone.** Unit/integration tests, fuzzing (cargo-fuzz), property-based testing (proptest), Miri, Kani for simple invariants, clippy discipline; writing a mini safety case; **capstone presentations + peer review**; wrap-up (Rust in Linux kernel, Android, cloud infra) | Fuzz the W2 parser; Kani-verify an arithmetic invariant; present capstone + safety report |

**Midterm checkpoint (end of W3 / into W4):** quiz on W1–3 + first project milestone.

---

## Projects *(compressed to the 7-week timeline)*

- **Continuous mini-labs** (weekly, auto-graded): build fluency and idiom.
- **Midterm project (W2–4, 20%):** a library component with documented invariants, full test suite, no `unsafe` (or a justified bounded surface). Examples: memory pool, safe config-parser, thread-safe key-value store.
- **Capstone (W4–7, 50%)** — choose one track (scope ~60–80% of the 15-week version):
  1. **Legacy migration:** reimplement a C/C++ service component in Rust, interoperate via FFI, document the safety delta
  2. **Protocol safety:** wire-format parser (e.g., HTTP subset, CSV/binary codec) hardened with fuzzing + property tests + safety case
  3. **High-performance service:** async HTTP/WebSocket service (e.g., rate-limiter, JSON API) benchmarked against a C++ reference, with load testing
  4. **Safety audit:** analyze an open-source C++ codebase, quantify UB/unsafety risk against CWE classes, present a Rust redesign with API sketch

  Each capstone: clippy-clean code, tests + fuzz results, and a **3–5 page** report with an explicit `unsafe` audit section.

---

## Assessment

| Component | Weight |
|-----------|--------|
| Weekly labs + quizzes | 15% |
| Midterm (quiz + project milestone) | 20% |
| Capstone (code + safety report + presentation) | 50% |
| Peer code review | 15% |

---

## Core Reading & References

- *The Rust Programming Language* (2nd ed.) — primary text
- *Rust by Example*, *The Rustonomicon* (unsafe), *Rust API Guidelines*
- C++20/23 standard library headers for side-by-side contrast; a shared **C++→Rust cheat sheet** (provided W1)
- **Security context:** CERT C Secure Coding, CWE Top 25, Rust Security Guidelines, supply-chain guidance (cargo audit docs)
- **Tooling docs:** Clippy, Miri, cargo-fuzz, proptest, Kani, cxx/cbindgen
- Selected: *The Cost of Memory Safety* (2020), Rust-in-Linux-Kernel docs

---

## Teaching Notes (7-week compression strategy)

- **Protect the differentiators.** Weeks 4–6 (unsafe/audit, FFI, security context) are what separate this from "Rust 101" — do not cut them; compress breadth topics instead.
- **Shift load to labs.** With only 7 weeks, lectures carry the concept; the 5 h lab does the fluency-building. Expect the **borrow-checker wall in W3** — the dedicated lab slot is non-negotiable; keep office hours open.
- **Keep `unsafe` honest.** The invariant-documentation + audit exercise (W4) is the pedagogical centerpiece; don't let students skip the "why is this safe" write-up.
- **Pin the toolchain** (edition 2021/2024) on day one to avoid version drift across the short module.
- **Capstone scoping:** with a ~3.5-week build window, require a working skeleton by the W4 checkpoint so students aren't building from scratch in W6–7.

---

**What changed vs. the previous version:** embedded track & `no_std` lab removed (W6); W6 now pairs FFI with a general-purpose security/supply-chain unit; learning outcomes 5–7 re-scoped to services, FFI, and CWE/CERT context; capstone track 2 replaced with a high-performance async service track; reading list swapped ISO 26262/MISRA for CERT C / CWE / Rust Security Guidelines.

Want me to generate the **W1 C++→Rust cheat sheet** or the **W4 unsafe-audit lab spec** next?