# CLAUDE.md — Iris

Read these before making changes:
- @PRD.md
- @DESIGN.md
- @ARCHITECTURE.md

## Strict Security & Engineering Constraints
- **Never write camera frames to disk.** Process `CVPixelBuffer` in RAM only.
- **Never store plaintext passwords or embeddings.** Always encrypt with `AES.GCM` (256-bit).
- **Hold the AES key in memory only during an active Touch ID session,** and zero it on timeout.
- **Enforce all 5 unlock gates** before synthesizing any keystroke via Accessibility.
- **Keep Face Recognition and Liveness Detection decoupled.** They are separate modules with separate inputs and outputs.
- **Follow Apple's macOS Sequoia Human Interface Guidelines:** native materials, SF Symbols, spring animations, and zero third-party UI bloat.

## Project Status
- Phase 0: repository, specs, and directory scaffolding. No Swift application code yet.
- Module folders under `iris/` are placeholders tracked with `.gitkeep`.
