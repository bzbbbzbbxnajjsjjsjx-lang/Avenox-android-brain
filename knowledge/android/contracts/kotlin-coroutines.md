---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Kotlin and coroutine lifecycle contract

- **Purpose and scope:** Bound asynchronous work, cancellation, and error handling to the lifecycle of the feature or operation that owns it.
- **Authority:** The mandatory rules are repository policy. Kotlin/Android coroutine pages are upstream recommendations; exact APIs and behavior must be checked against selected Kotlin and AndroidX versions.
- **Mandatory project rules:** Do not use unscoped global work for feature operations. Every launched operation has an explicit owner and cancellation boundary. UI state holders expose lifecycle-appropriate observable state and do not leak an Activity, View, or composable scope into long-lived data components. Handle expected failures at a deliberate boundary; do not silently swallow cancellation. Keep dispatcher selection at the layer that performs blocking or CPU-intensive work, preferably injectable/testable when needed.
- **Recommended upstream practices:** Use structured concurrency, suspend functions for one-shot work, Flow for streams, and lifecycle-aware collection in UI. Prefer `viewModelScope` for work owned by a ViewModel and lifecycle-aware collection APIs where supported. Inject dispatchers when deterministic coroutine tests need control.
- **Evidence:** A1 [Kotlin coroutines on Android](https://developer.android.com/kotlin/coroutines), [coroutine best practices](https://developer.android.com/kotlin/coroutines/coroutines-best-practices), [lifecycle-aware coroutines](https://developer.android.com/topic/libraries/architecture/coroutines), and [Kotlin coroutine basics](https://kotlinlang.org/docs/coroutines-basics.html).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** API names and lifecycle integration vary by AndroidX Lifecycle and Kotlin versions. Confirm artifact/API availability in the relevant release record and project build before use. No Kotlin, Lifecycle, or coroutine dependency baseline exists here.
- **Verification:** Unit-test cancellation, completion, failure, and state transitions with a test dispatcher or controlled fake. Check that collection stops at the intended lifecycle boundary in instrumentation when lifecycle behavior is material.
- **Exceptions and trade-offs:** Work intentionally surviving a screen may be delegated to a longer-lived owner such as a repository or platform work scheduler, with explicit persistence/retry semantics. Do not extend UI scope merely to avoid cancellation.
