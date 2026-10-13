---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Performance and resource-efficiency contract

- **Purpose and scope:** Prevent avoidable UI stalls, excess work, memory pressure, and battery/network waste while using measurements to prioritize changes.
- **Authority:** Mandatory rules prohibit known unbounded or main-thread blocking behavior. Android performance material is upstream guidance, not a project-specific target budget.
- **Mandatory project rules:** Do not perform blocking I/O or unbounded expensive computation on the main thread. Bound caches, retained collections, retries, and background work according to product needs. Avoid repeated work in hot UI paths when the cost is established. A performance claim requires a reproducible measurement with device/API, build variant, scenario, and tool recorded.
- **Recommended upstream practices:** Profile representative user journeys before optimizing; use system traces, Macrobenchmark, Baseline Profiles, and memory/network tools where applicable. Prefer lazy UI for genuinely large collections and avoid unnecessary allocations/measurement passes. Set budgets from product requirements and measured baselines, not generic numbers copied from guidance.
- **Evidence:** A1 [Android performance](https://developer.android.com/topic/performance), [Compose performance](https://developer.android.com/develop/ui/compose/performance), [Compose best practices](https://developer.android.com/develop/ui/compose/performance/bestpractices), [app startup](https://developer.android.com/topic/performance/vitals/launch-time), and [Baseline Profiles](https://developer.android.com/topic/performance/baselineprofiles/overview).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** Tooling, profile formats, APIs, and thresholds vary with Android Gradle Plugin, AndroidX, OS, and device. Check versioned docs and release notes before adoption. No target API, device set, or performance budget is declared.
- **Verification:** Separate static inspection from measurements. Run profiling/benchmarks on documented representative hardware and release-like builds; report sample count, warm/cold conditions, variance, and baseline. SDK/device/emulator checks are not passed until actually run.
- **Exceptions and trade-offs:** Small bounded work may be acceptable on the caller thread when proven non-blocking and UX-safe. Optimization complexity must be supported by measured benefit and revisited after relevant architecture or platform changes.
