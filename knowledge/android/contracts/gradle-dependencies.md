---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
# Gradle dependency and compatibility contract

- **Purpose and scope:** Keep build inputs understandable, reproducible, and compatible with explicitly selected tool versions.
- **Authority:** Repository requirements are mandatory build hygiene. Gradle, Android, and Kotlin compatibility tables are authoritative upstream evidence for their stated versions; recommendations here do not choose a version tuple.
- **Mandatory project rules:** Declare plugin and dependency versions in one reviewable source of truth where practical; avoid unexplained dynamic versions and duplicate/conflicting declarations. Use the Gradle Wrapper for project builds once present. Record resolved versions when constraints, platforms, BOMs, substitutions, or locks affect selection. Before changing a plugin/tool version, check the exact AGP, Gradle, Kotlin Gradle Plugin, JDK, and relevant plugin compatibility documentation. Never infer support by interpolating a matrix.
- **Recommended upstream practices:** Use version catalogs for centralized coordinates where they improve consistency, dependency constraints/platforms for alignment, dependency locking when reproducibility needs it, and dependency verification for supply-chain integrity when the project configures it. Update related tools together only according to documented constraints and tested evidence.
- **Evidence:** A1 [Android dependency management](https://developer.android.com/build/dependencies), [AGP release notes](https://developer.android.com/build/releases/about-agp), [Gradle versioned user manual](https://docs.gradle.org/current/userguide/userguide.html), [Kotlin Gradle compatibility](https://kotlinlang.org/docs/gradle-configure-project.html), and [Android JDK guidance](https://developer.android.com/build/jdks).
- **Session verification status:** The linked pages were not independently revalidated in this session. The Cloud proxy denied HTTPS CONNECT with 403 for `developer.android.com`, `kotlinlang.org`, `docs.gradle.org`, and `android.googlesource.com`; the links are canonical references, not claims of page review.
- **Applicability and version limits:** The Gradle `current` URL moves; use `https://docs.gradle.org/<wrapper-version>/userguide/` for project claims. Compatibility depends on exact versions and plugins. No build files, wrapper, or version catalog exist at this baseline; no matrix is asserted.
- **Verification:** Inspect plugin management, wrapper distribution, version catalog, constraints, lock/verification metadata, resolved dependency report, and CI JDK. Run configuration/build checks only with authorized installed tools and available SDK/dependencies; report exact command and result.
- **Exceptions and trade-offs:** A narrowly scoped explicit version override may be justified to work around an upstream constraint; document the reason, affected artifact, compatibility evidence, and removal trigger. Do not add catalogs/locking machinery before the build has a concrete need.
