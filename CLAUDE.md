# CLAUDE.md — verismo

VeriSMo: a formally verified security module for AMD confidential VMs (SEV-SNP), implemented
in Rust with Verus formal specifications. Research prototype (2022–2024). Used by Project-Metic
as a reference implementation for low-level verified Rust and as a verification target for the
Metic binary lifting and Verus proof synthesis pipelines.

## What This Repo Is

A research-grade verified security module — not a production service. The codebase demonstrates
what is possible for verified low-level Rust (kernel-like code) using the Verus toolchain
(circa 2022–2024). It will not compile with the latest Verus; the toolchain version is pinned
in `tools/`.

**Do not attempt to update this to the latest Verus without a deliberate research effort.**
See the README for the specific toolchain version constraints.

## Key Invariants

- **This is a research prototype.** Do not deploy as production security infrastructure.
- **Toolchain version is frozen.** The Verus version is pinned. Updating requires proof
  re-engineering that is out of scope for routine maintenance.
- **Separation of verified and unverified code.** `source/verismo/` contains verified code;
  `source/verismo_main/` contains only the unverified panic handler entry point.
  `source/verismo/src/entry.s` is explicitly unverified assembly.
- **No modifications to verified proofs without full re-verification.** Any change to
  `source/verismo/` requires running the Verus verifier to completion with zero errors.

## Dev Commands

```bash
# Install the pinned Rust + Verus toolchain
tools/install.sh

# Build
cargo build

# Verify (requires Verus — run after toolchain install)
# See tools/ for the verification invocation
```

## Tech Stack

- Rust (no_std), Verus (formal verification), AMD SEV-SNP APIs

## Integration with Project-Metic

- Source of binary lifting test cases for AMD64 verified kernel code
- Reference target for `verus-proof-synthesis` LLM-based proof generation
- Example of Verus-verified low-level code for `VeriStruct` benchmark development

## What NOT to Do

- Do not use this as a production security module
- Do not update the Verus toolchain version without a dedicated re-verification effort
- Do not modify verified proof code in `source/verismo/` without re-running the verifier
