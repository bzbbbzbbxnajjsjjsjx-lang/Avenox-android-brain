---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Jetpack Compose state and effects contract

- **Purpose and scope:** Make UI state ownership, recomposition, and imperative effects explicit and safe.
- **Authority:** Repository invariants are mandatory. Compose guidance is upstream recommendation; APIs marked experimental or version-sensitive require exact-version review.
- **Mandatory project rules:** A composable must remain safe to re-enter and recompute; do not perform externally visible one-time work directly in a composable body. State has one identifiable owner and flows down while events flow to that owner. Keep mutable state observable by Compose; avoid hidden mutation that cannot trigger the intended UI update. Key remembered values and effects to the inputs that define their identity. Never launch unmanaged work as a side effect of recomposition.
- **Recommended upstream practices:** Hoist state to the lowest common owner that needs to read/write it. Use `remember` for composition-scoped retention and `rememberSaveable` only for saveable UI state that should survive supported recreation. Choose effect APIs by lifecycle semantics (`LaunchedEffect`, `DisposableEffect`, `SideEffect`, or `rememberCoroutineScope`) and use `derivedStateOf`/deferred reads only when they reduce meaningful work.
- **Evidence:** A1 [Compose state](https://developer.android.com/develop/ui/compose/state), [side effects](https://developer.android.com/develop/ui/compose/side-effects), [thinking in Compose](https://developer.android.com/develop/ui/compose/mental-model), and [performance best practices](https://developer.android.com/develop/ui/compose/performance/bestpractices).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** Compose API details and stability annotations vary by Compose version. Consult the project’s exact Compose BOM/library mapping and release notes; none is declared in this repository.
- **Verification:** Test rendered state and user events with Compose UI tests; unit-test state holders separately. Use recomposition/layout tracing or benchmarks when a measured performance concern justifies it. Static review checks that effects have deliberate keys and disposal behavior.
- **Exceptions and trade-offs:** A composable may perform pure calculations and construct UI descriptions. Remembering or deferring work is not automatically faster; keep straightforward code unless measurement or documented semantics justify added complexity.
