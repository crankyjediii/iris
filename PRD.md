# Product Requirements — Iris

## Overview
- Build **Iris**, an on-device face unlock utility for macOS 15 (Sequoia) and later that unlocks a Mac at the lock screen when the user glances at it, without typing.
- Face recognition and liveness checks run entirely on-device using Apple's Vision and Core ML frameworks.
- The interface lives in the MacBook notch as an Apple-native, Dynamic Island-style animation with clear scanning, success, and failure states.

## Users
- Mac owners who want quick, hands-free unlock for their personal machine.
- Users who understand this is a convenience feature, not a security upgrade over Touch ID or Face ID.

## Core Features
- **9-Direction Face Enrollment:** Guided capture across nine head directions. Each frame becomes a 512-number embedding (stored alongside a normalized centroid), and the image is immediately discarded.
- **Multiple Identities:** Enroll several profiles (e.g., with or without glasses) and toggle each one on or off without deleting it.
- **Flexible Triggers:** Unlock on wake, on lock, or on pressing Space at the lock screen, in any combination.
- **Liveness Checks:** Off, Light (deny cues only: screen glare and device rectangles), or Heavy (deny cues + 3D confirm cues: landmark perspective, nose parallax, and blinks over a 2-second window, with a subtle micro-prompt if the user is motionless after 1.2s).
- **Notch UI & Retry:** A pitch-black closed pill that expands into a scan animation, with hover-to-retry (with trackpad haptics), a 3.5-second scan timeout, 3-strike auto-backoff, and an option to turn animations off.
- **Smart Camera Selection:** Set separately for the built-in display and an external monitor, automatically switching based on clamshell/display state.
- **Sliding Auto-Locking Session:** Touch ID authorization holds the AES key in memory and re-locks after a user-chosen sliding idle period.
- **Notchless Mac Fallback:** A consistent Dynamic Island-style pill anchored to the top center of notchless screens.
- **Credential Storage:** The Mac password is encrypted with a 256-bit AES-GCM key held in the macOS Keychain and protected by `userPresence`.
- **Data Control:** Edit or delete enrollments and the stored password at any time, with encrypted files removed from disk immediately.
- **5-Gate Unlock Gating:** Typing the password requires a valid active session, the Mac confirmed at the lock screen, a matching enabled identity above threshold, liveness acceptance, and Accessibility permission.
