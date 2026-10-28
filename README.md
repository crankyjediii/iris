# Iris

**Face unlock for your Mac, handled entirely on-device.**

Iris unlocks your Mac at the lock screen when you glance at it. No typing. The interface lives in the MacBook notch as a black pill that expands into a scan animation, confirms, and gets out of the way.

> **Iris is a convenience feature, not a security upgrade.** It does not replace Touch ID or Face ID, and it is not hardened against determined spoofing. Keep a strong Mac password for anything sensitive. Onboarding and Settings make these limits explicit.

## How It Works
- **On-device only.** Face detection (Apple Vision), embeddings (a 512-dimensional ArcFace model run through Core ML), and liveness checks all run locally. Camera frames are processed in RAM and never written to disk.
- **Encrypted at rest.** Face embeddings and your Mac password are encrypted with AES-GCM (256-bit). The master key lives in the macOS Keychain and requires user presence to access.
- **Five gates before typing.** Iris types your password only when an active session exists, the Mac is confirmed at the lock screen, a matching enabled identity clears the threshold, liveness passes, and Accessibility permission is granted.
- **Triggers you choose.** Unlock on wake, on lock, or on pressing Space at the lock screen, in any combination.

Full specifications: [PRD.md](PRD.md), [DESIGN.md](DESIGN.md), [ARCHITECTURE.md](ARCHITECTURE.md).

## Requirements
- macOS 15 (Sequoia) or later
- Apple Silicon or Intel Mac
- Xcode 26 or later

## Building
The Xcode project arrives in Phase 1. Until then this repository holds the specifications and the module layout only, so there is nothing to build yet.

Once the project exists, open it in Xcode 26 or later, select the app scheme and your Mac, then run. Grant Camera and Accessibility permissions when prompted. The Core ML model is produced by the tooling in [`tools/`](tools/README.md).

## Repository Layout
```
iris/             Swift sources, one folder per module
tools/            Model conversion and tooling
PRD.md            Product requirements
DESIGN.md         Visual and interaction design
ARCHITECTURE.md   Technical architecture
```

## Acknowledgements
- **The Boring Notch** for notch window physics.
- **InsightFace** for the ArcFace model architecture.
- **Alcove** for HUD design inspiration.
- **Glance** for concept inspiration.

## License
MIT. See [LICENSE](LICENSE).
