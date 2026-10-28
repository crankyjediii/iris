# Design — Iris

## Overview
- Iris brings the first-party Face ID feel of an iPhone to the Mac. A glance unlocks the Mac with no typing.
- Every element follows Apple's macOS Sequoia Human Interface Guidelines—using native materials, SF Symbols, and spring physics so it feels built into macOS.
- The app stays out of the way until the Mac locks or wakes, appearing only when needed.
- The design is transparent about its limits: Iris is a convenience feature, not a hardware security upgrade, and onboarding/settings make this explicit.

## Visual Direction
- **Hardware-Blended Notch Pill:** Uses pure black (`#000000`) and continuous squircle geometry to merge invisibly with the physical MacBook camera housing.
- **Fluid Spring Motion:** Transitions between `closed`, `scanning`, `success`, and `failure` states use continuous spring physics (`stiffness: 280, damping: 24`) and native SF Symbol animations (`.pulse`, `.bounce`, `.replace`) so the island reads as one living shape.
- **Notchless Fallback:** Macs without a notch and external monitors render the exact same pill shape anchored at the top center of the display.
- **Native Sequoia Settings:** Standard `NavigationSplitView` with `.ultraThinMaterial` sidebar vibrancy, `.formStyle(.grouped)`, SF Pro typography, and semantic system colors.

## Layout and Components
- **Notch Pill:** Closed, scanning (with optional subtle liveness micro-prompt in Heavy mode), success, failure, and paused (after 3 failed auto-attempts) states.
- **Onboarding Assistant:** 4-step native setup window (Security Disclaimer → Permissions → 9-Point Face ID Ring Enrollment → Password Vault).
- **Settings Window:** Identity list with per-identity toggles, camera selection (separate for built-in and external displays), recognition similarity slider, liveness strictness (`Off`, `Light`, `Heavy`), trigger checkboxes, and sliding idle timeout for the Touch ID session.
- **Face Lab:** Hidden debug inspector in the sidebar, unlocked by clicking the app icon 5 times in `Settings → About`.
- **Data Controls:** Immediate, permanent deletion for individual face profiles or the stored password with clear pre-confirmation copy.

## Interaction Principles
- Users choose their triggers (`Wake`, `Lock`, or `Space at Lock Screen`) in any combination.
- Hovering over the notch immediately retries a failed or paused scan, accompanied by `.alignment` trackpad haptic feedback.
- Animations can be turned off completely so Iris operates invisibly.
- Failure is calm and never blocks manual typing at the lock screen.
