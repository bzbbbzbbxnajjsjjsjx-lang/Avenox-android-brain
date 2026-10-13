---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Android engineering contracts

These contracts set reviewable repository policy for future Android code. They distinguish mandatory project invariants from upstream recommendations. Official links are canonical starting points; version-sensitive APIs and compatibility must be rechecked against the exact project tool/library versions and the [source registry](../sources/registry.md). No Android app or version baseline is defined at the current repository baseline.

## Contract inventory

- [Application architecture](architecture.md)
- [Kotlin and coroutine lifecycle](kotlin-coroutines.md)
- [Compose state, recomposition, and effects](compose-state-effects.md)
- [Navigation and adaptive layouts](navigation-adaptive.md)
- [Material 3 and accessibility](material-accessibility.md)
- [Performance and resource efficiency](performance-efficiency.md)
- [Gradle dependencies and compatibility](gradle-dependencies.md)
- [Testing, build verification, and release quality](testing-release-quality.md)

## Quality gate categories

The testing contract requires separate evidence for static checks, unit tests, instrumented/UI tests, accessibility, performance measurements, and build/artifact validation. Report each as `passed`, `failed`, `not run`, or `not applicable`, with command and environment. Android SDK-, emulator-, device-, signing-, or service-dependent checks are **not passed until executed successfully**. The gate is a reporting contract; it does not claim current CI, hardware, SDK installation, or thresholds.

## Evidence and freshness

Sources carry registry authority: A1 official documentation/release records, A2 pinned implementation source, A3 illustrative samples. These contracts cite A1 pages only. Links were selected as canonical references; no linked upstream page was independently revalidated in this review session. The enforced restricted Cloud proxy returned HTTP 403 during HTTPS CONNECT to `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`. No direct-route or proxy bypass was attempted. Do not treat these notes as a dated review of current API availability or compatibility.
