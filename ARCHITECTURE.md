# Architecture — Iris

## Overview
- Build a macOS menu-bar-adjacent app (`LSUIElement`) that unlocks a Mac by recognizing the user's face at the lock screen.
- Run all face detection, embedding extraction, and liveness verification 100% on-device. No camera frames, face data, or credentials ever leave the machine or touch the disk unencrypted.
- Target macOS 15 or later on Apple Silicon and Intel.

## Tech Stack
- **UI & Windowing:** Swift 6, SwiftUI, and AppKit (`NSPanel` with `.nonactivatingPanel` and `.canJoinAllSpaces` for lock-screen notch rendering).
- **Vision:** Apple `Vision` framework for face rectangles, facial landmarks, and rectangular device-border detection.
- **Core ML:** FP16 ArcFace model (`ArcFace512.mlpackage`) producing 512-dimensional L2-normalized embeddings.
- **Cryptography:** `CryptoKit` `AES.GCM` (256-bit) for encrypting embeddings and the stored password.
- **Keychain:** macOS Keychain storing the 256-bit master key with `.userPresence` access control.
- **System Locking & Input:** `CGSessionCopyCurrentDictionary` + `DistributedNotificationCenter` for lock-screen verification, and Accessibility `CGEvent` for typing the password.

## Main Modules
- **Enrollment:** Guides the user through 9 head directions, validates pose angles via Vision landmarks, extracts 512-d embeddings + a normalized centroid, encrypts to disk, and discards all frames from RAM.
- **Identity Store:** Manages encrypted embedding bundles for multiple identities, each individually toggleable.
- **Recognition:** Computes cosine similarity between live embeddings and enabled identities (taking the max across the 9 directional vectors and centroid) against a configurable threshold.
- **Liveness:** Evaluates a ~2.0-second rolling window of frames independently from recognition. Deny cues (screen glare, rectangular phone/tablet borders) fail a scan outright. Confirm cues (3D landmark geometry, nose-tip parallax across head turns, EAR blinks) confirm a live face in Heavy mode.
- **Session Manager:** Authorizes a session via Touch ID, holds the 256-bit AES key in RAM only while active, resets the sliding timer on each unlock, and zeroes the key on idle timeout.
- **Notch UI:** Anchors the Dynamic Island pill to the physical notch or top screen edge, handling state transitions, hover retry, haptics, and 3-strike backoff.
- **Face Lab:** Hidden debug console exposing real-time embeddings, similarity scores, and liveness cue metrics.

## Data Flow
1. **Enrollment:** `Camera CVPixelBuffer (RAM)` → `Vision Landmarks & Pose Check` → `Core ML 512-d Embedding` → `Compute 9 Vectors + Centroid` → `AES-GCM Encrypt & Save` → `Discard Frame`.
2. **Unlock:** `Lock/Wake/Space Event` → `Verify Active Session & Lock Screen State` → `Select Camera (Built-in vs. Clamshell External)` → `Notch Expands to Scanning (3.5s max)` → `Parallel Check: [Max Cosine Similarity >= Threshold] && [Liveness Window Accepted]` → `Decrypt Password in RAM` → `Accessibility Synthesizes Keystrokes + Return` → `Zero Plaintext Buffer & Reset Sliding Session Timer`.
