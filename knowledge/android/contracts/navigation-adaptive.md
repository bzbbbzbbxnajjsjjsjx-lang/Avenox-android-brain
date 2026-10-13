---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Navigation and adaptive layout contract

- **Purpose and scope:** Keep destinations, back behavior, and layouts coherent across window sizes and input modes.
- **Authority:** Mandatory project rules specify observable behavior; Android navigation and adaptive guidance are upstream recommendations. No navigation library is selected by this contract.
- **Mandatory project rules:** Navigation state and back behavior must be deliberate and testable. Do not encode navigation as scattered screen-local booleans when a destination/back stack model is required. Layouts must not assume one fixed width or orientation; critical content and actions remain available as the window changes. Preserve user task state where the selected navigation behavior requires it.
- **Recommended upstream practices:** Choose a navigation library based on app needs and supported versions; evaluate adaptive scaffolds or list-detail patterns against window size classes and product flows. Separate destination state from adaptive presentation so layout changes do not accidentally duplicate navigation state. Follow current Android adaptive design guidance for foldables, resizing, and large screens.
- **Evidence:** A1 [Navigation 3](https://developer.android.com/guide/navigation/navigation-3), [Navigation component](https://developer.android.com/guide/navigation), [build adaptive apps](https://developer.android.com/develop/ui/compose/build-adaptive-apps), and [adaptive navigation](https://developer.android.com/develop/adaptive-apps/guides/build-adaptive-navigation).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** Navigation 3, Navigation component, window-size APIs, and adaptive material artifacts have independent versions and release channels. Check exact AndroidX release records; do not infer adoption or compatibility from these links. No app or library baseline exists.
- **Verification:** Unit-test route/state transitions; UI-test primary journeys and back behavior; use resizable emulator/device configurations at representative compact and expanded widths when available. Record dimensions/API/device and distinguish test coverage from all-device guarantees.
- **Exceptions and trade-offs:** A single-screen utility may not need a navigation framework. If a product intentionally targets one device class, document that scope and still handle supported resizing, accessibility, and configuration changes.
