# Cross-Platform Architecture & Engineering

> Distilled engineering knowledge for designing software that spans web, mobile, desktop, and embedded platforms.

Building for multiple platforms requires distinct strategic decisions. Sharing code reduces development time, but over-sharing compromises the native user experience. The goal is to maximize shared logic while respecting the platform's UI conventions.

---

# Core Principles

Every cross-platform strategy should satisfy:

**Logic is Universal, UI is Local:** Business logic, state management, and API communication should be shared. The User Interface should respect the platform (e.g., iOS navigation feels different than Android navigation).
**Offline First (Usually):** Mobile and desktop apps are expected to function, at least partially, without an internet connection. Web apps should strive for this via Service Workers.
**Graceful Degradation:** If a device lacks a feature (e.g., a camera or a specific biometric sensor), the application must handle it without crashing.

---

# Architectural Approaches

Choose the approach based on budget, performance needs, and team expertise.

**1. Progressive Web Apps (PWA):**
- **What:** Standard web applications enhanced with Service Workers and Web App Manifests to allow installation and offline capabilities.
- **Pros:** Single codebase, bypasses App Store reviews, instantly updateable.
- **Cons:** Limited access to deep native APIs (Bluetooth, background tasks), poor support on iOS.

**2. Web View Wrappers (Capacitor / Cordova):**
- **What:** A web application running inside a native shell's browser component.
- **Pros:** Fast time-to-market for web teams, access to native APIs via plugins.
- **Cons:** UI rendering is tied to the web engine, which can feel sluggish compared to native rendering.

**3. Cross-Platform Native (React Native / Flutter):**
- **What:** Code written in JS/Dart that compiles to or orchestrates native UI components.
- **Pros:** Near-native performance, excellent tooling, massive code sharing.
- **Cons:** Requires understanding the underlying native build systems (Xcode/Gradle) when things break.

**4. Pure Native (Swift/Kotlin):**
- **What:** Distinct codebases written in the platform's official languages.
- **Pros:** Uncompromised performance, immediate access to new OS features.
- **Cons:** Requires multiple specialized engineering teams, highest cost.

---

# State Management and Data Sync

Cross-platform apps often require robust synchronization.

**Local First:** Write data to a local database first (e.g., SQLite, WatermelonDB), then sync to the server in the background. This makes the UI feel instantly responsive regardless of network conditions.
**Conflict Resolution:** When syncing offline changes, the server must have a strategy (Last Write Wins, CRDTs, or manual resolution) to handle conflicts.

---

# Design Systems Across Platforms

A design system must adapt to the platform context.

**Platform Nuances:**
- *Web:* Hover states exist. Click targets can be smaller.
- *Mobile:* No hover states. Click targets must be at least 44x44pt. Use platform-specific navigation (Bottom tabs on iOS, Drawer or Bottom tabs on Android).
- *Desktop:* Expect heavy keyboard usage, right-click context menus, and multi-window support.

**Design Tokens:** Share design tokens (colors, spacing, typography) via JSON so both the web CSS and the mobile styling engine (e.g., React Native StyleSheet) pull from the exact same source of truth.

---

# The ForgeCore Cross-Platform Standard

A production-ready cross-platform application is:
- **Context-Aware:** It feels like an iOS app on an iPhone, and a web app in Chrome.
- **Resilient:** It handles network transitions (online to offline) seamlessly.
- **Efficient:** It shares as much non-UI code as possible to minimize bugs and maximize feature velocity.
