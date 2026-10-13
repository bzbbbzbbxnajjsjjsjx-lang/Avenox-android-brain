---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Android application architecture contract

- **Purpose and scope:** Keep Android UI, domain behavior, and data access understandable and independently verifiable as application modules are introduced.
- **Authority:** Project rules below are mandatory for this repository when Android code exists. Linked Android recommendations are upstream guidance, not a claim that this repository has adopted a particular architecture or library.
- **Mandatory project rules:** UI rendering code must not own durable data access or business policy. Keep Android framework and persistence/network details behind boundaries where they can be replaced or tested. A layer or abstraction is added only for a concrete responsibility, reuse need, or test seam; do not create empty layers, repositories, or use cases by template. State ownership and write paths must be identifiable for each feature. Document intentional boundary exceptions in the relevant decision note.
- **Recommended upstream practices:** Start with a data layer and UI layer; add a domain layer when business logic is reused or makes a UI state holder too complex. Prefer unidirectional data flow, observable UI state, and dependency injection where object graph complexity justifies it. These are recommendations from Android architecture guidance; the project has not selected a DI framework.
- **Evidence:** A1 [Android architecture](https://developer.android.com/topic/architecture), [architecture recommendations](https://developer.android.com/topic/architecture/recommendations), and [UI layer](https://developer.android.com/topic/architecture/ui-layer).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** General guidance, not a framework compatibility contract. Recheck pages and library versions when choosing APIs. No app modules or Android version baseline are present at the contract baseline.
- **Verification:** Review dependency direction and ownership; unit-test business policy without Android UI where feasible; verify state and data flow with feature tests. Static module dependency checks may be added only when the project has modules and a declared rule to enforce.
- **Exceptions and trade-offs:** A small feature may keep simple transformation logic near its state holder. Avoid abstraction cost when there is one clear owner and no independent test boundary. Record exceptions that cross a mandatory boundary with scope and rationale.
