---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Material 3 and accessibility contract

- **Purpose and scope:** Use a coherent visual system while keeping content and interactions perceivable and operable.
- **Authority:** Accessibility invariants below are mandatory project quality requirements; Material guidance is upstream design recommendation. This contract does not require a particular Material artifact or theme implementation.
- **Mandatory project rules:** Do not communicate meaning by color alone. Interactive controls need an accessible name, role, and state; touch targets, text scaling, contrast, focus, and screen-reader traversal must be checked for each relevant flow. Decorative content should not create confusing accessibility nodes. User-facing motion or animation must not block operation and must honor applicable reduced-motion platform behavior where exposed by the chosen APIs.
- **Recommended upstream practices:** Follow Material 3 guidance for color roles, typography, shape, components, and adaptive patterns; use semantic Compose components before custom drawing or interaction. Prefer theme tokens and semantic properties over hard-coded visual values. Compose semantics can merge or clear nodes, so inspect the resulting accessibility tree.
- **Evidence:** A1 [Material 3 for Compose](https://developer.android.com/develop/ui/compose/designsystems/material3), [Compose accessibility](https://developer.android.com/develop/ui/compose/accessibility), [Android accessibility principles](https://developer.android.com/guide/topics/ui/accessibility/principles), and [large font support](https://developer.android.com/develop/ui/compose/accessibility/scalable-content).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** Material component APIs and defaults are artifact-version-specific; visual guidance does not prove that a library version is stable or included by a BOM. Recheck exact AndroidX Material3 release notes. No Material version is established.
- **Verification:** Combine automated accessibility checks where supported with manual TalkBack/navigation, font scaling, contrast, focus-order, and target-size review on representative configurations. Capture tool/device/API and exceptions; automated checks alone are insufficient.
- **Exceptions and trade-offs:** Brand or platform constraints may justify a documented visual deviation, but not removal of equivalent accessible meaning or operation. Custom components require explicit semantics and interaction testing.
