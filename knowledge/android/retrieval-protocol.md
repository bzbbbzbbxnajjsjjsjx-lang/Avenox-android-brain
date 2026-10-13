---
{
  "kind": "note",
  "project": "avenox-android",
  "visibility": "internal"
}
---
Android engineering retrieval protocol

This protocol uses the official Avenox context and sync CLI plus the Android source registry in `knowledge/android/sources/registry.md`. It does not assume lifecycle hooks, a custom index, or a project version baseline. The registry and examples guide source selection; they are not evidence that a given API or version is adopted by this repository.

## Deterministic workflow

1. **Frame the question.** State the decision or technical claim to resolve, affected module/API, Android form factors or platform level, and whether the question is about documented behavior, compatibility, implementation details, or a sample pattern. Split questions that require different evidence.
2. **Read the project’s actual inputs.** When build files exist, inspect `settings.gradle[.kts]`, root and module `build.gradle[.kts]`, `gradle/libs.versions.toml`, `gradle/wrapper/gradle-wrapper.properties`, relevant `gradle.properties`, convention plugins/build logic, dependency locks, and CI build/JDK declarations. Follow aliases and plugin management to their definitions. Record declared values separately from resolved values. Distinguish the JDK that runs Gradle from Java/Kotlin toolchains and bytecode targets. Do not launch a wrapper or build if it would download a missing distribution or dependencies without authorization. If no build inputs exist, write `project baseline: unknown / not present`.
3. **Retrieve project memory explicitly.** Run the official Avenox `context` command in the next section for the project and question. Read the cited source when the returned excerpt is truncated or insufficient. A missing result means no indexed memory was found; it does not prove a technical claim false.
4. **Choose the smallest relevant source set.** Select only the registry entries that cover the question. For a library-version question, use its artifact’s AndroidX release record; for build compatibility, use the matching Kotlin, AGP, Gradle, and JDK compatibility records. Add implementation source only for a source-level question. Use samples only when comparing an illustrative implementation pattern.
5. **Verify current upstream evidence.** Open the canonical, version-appropriate A1 page and release/compatibility record. Record the artifact or tool version, stable/preview channel, page heading or table, and retrieval date. For moving A2 branches, pin a full commit SHA and path. Recheck pages for every version-sensitive answer; the registry’s URLs and Avenox memories do not freeze upstream facts.
6. **Resolve scope and conflicts.** Compare the exact versions, release channels, API levels, platforms, and dates each source covers. Prefer the most specific applicable A1 record for normative or compatibility claims. Use A2 to explain observed implementation, not to expand a public contract. A3 cannot override A1. If primary sources disagree or leave a gap, state both scopes and leave the conclusion unresolved until the gap is closed.
7. **Write a bounded conclusion.** Separate observed project declarations, verified upstream facts, inference, and unknowns. Include provenance, version scope, confidence, and any build/test still needed. Never convert “not found” or an untested build into compatibility evidence.
8. **Persist selectively.** Save only a durable project decision or a lesson verified by a named test or exact upstream revision. Use the decision or verified-lesson template. Keep the source links and version conditions with the note. Do not store transient answers, hypotheses, bulk documentation, or inferred project preferences. Sync after changing a Markdown source.

## Authority and selection rules

| Level | Use for | Required handling |
|---|---|---|
| **A1** | Official Android/Kotlin/Gradle documentation, release records, and compatibility guidance. | Select the exact topic and version/release channel. Use for normative statements and documented support ranges. An official page outside its stated scope is not evidence for the question. |
| **A2** | Upstream implementation source. | Pin repository, full commit SHA/tag, and relevant path. Use only to explain implementation behavior at that revision; it does not establish a supported API contract. |
| **A3** | Official Android samples. | Pin repository commit and file. Treat as an example of one implementation, never a specification, compatibility guarantee, or project decision. |

Use no source merely because it is in the registry. Typical minimal sets: Compose API behavior → relevant A1 Compose page; AndroidX dependency version → artifact release page plus the relevant A1 usage page; Kotlin/AGP/Gradle/JDK tuple → Kotlin compatibility data, exact AGP release notes, versioned Gradle Java compatibility, and Android JDK guidance; visual design intent → Material 3 guidance, adding artifact release/BOM mapping only when API/dependency versions matter.

## Safeguards

- **Stale guidance:** Reopen the exact upstream page for each version-sensitive answer. Record `checked_on`; use versioned documentation when available. Do not treat a stored memory, search snippet, `current` URL, or old sample as current evidence.
- **Stable versus preview:** Preserve the channel exactly: stable, RC, beta, alpha, preview, or unspecified. Never strip a suffix or promote a preview to stable. Check the artifact-specific release page, not a family name alone.
- **Compatibility:** Compare declared project versions with the exact official compatibility range. A range intersection means only that the cited documentation describes support; it does not prove the project’s full plugin graph builds. Report unknown for absent versions, unlisted pairs, missing evidence, or ambiguity. Do not fill gaps by interpolation.
- **Irrelevant context:** Scope the Avenox query to `avenox-android`, use a narrow question and bounded result count, and inspect only cited sources. A context result is supporting memory, not upstream proof. Do not feed unrelated notes or whole-document dumps into the answer.
- **Unverified assumptions:** Label hypothetical inputs and inferences. State what was inspected and what was not. Do not claim an installed SDK, resolved dependency, test result, hook event, API maturity, or runtime behavior without direct evidence.
- **Conflicting guidance:** Keep claims side by side with their dates, versions, and scopes. Explain a resolvable difference; otherwise say `unresolved` and avoid a recommendation that depends on choosing one interpretation.

## Manual Avenox retrieval and sync

Use the repository root containing `beyin.py`. Do not rely on Codex lifecycle hooks for this workflow.

```sh
python3 -B beyin.py doctor
python3 -B beyin.py sync
python3 -B beyin.py context "<focused question and relevant version terms>" \
  --project avenox-android --harness codex --limit 6 --budget-chars 6000 --json
```

Before sync, inspect `doctor.state_location`: the effective state should resolve to the configured writable runtime, be installed there, and remain outside the repository. A successful sync with no warnings or conflicts is the write check for this run. The repository’s `.beyin-runtime.json` is a machine-local binding, not a portable project setting; do not copy another machine’s path into Git. SQLite and machine-specific state belong in the configured runtime, not the repository.

If the state pin is missing/mismatched or the runtime is not writable, do not move it, delete it, change permissions, use elevated privileges, or bypass the sandbox. Do not claim a fresh sync. If an existing index is readable, use the official read-only form:

```sh
python3 -B beyin.py context "<focused question>" \
  --project avenox-android --harness codex --limit 6 --budget-chars 6000 --no-sync --json
```

Mark that result as potentially stale. If read-only context also fails, inspect only the relevant tracked Markdown source directly and report that Avenox retrieval was unavailable. Do not fabricate a memory result. Resume synced retrieval only after the supported runtime is writable. No Jev, remote advisor, database, or custom retrieval service is required.

## Evidence format

Use this compact record in future engineering reports; omit empty fields rather than guessing:

```yaml
question: "<bounded claim or decision>"
project_baseline:
  observed: { "AGP": "<declared|unknown>", "Gradle": "<declared|unknown>", "KGP": "<declared|unknown>", "JDK-running-Gradle": "<observed|unknown>" }
evidence:
  - authority: A1
    source_id: <registry-id>
    url: <canonical-page>
    location: <heading, artifact, table, or file path>
    version_channel: <exact version and stable|rc|beta|alpha|preview|unknown>
    checked_on: YYYY-MM-DD
    claim: <narrowly supported statement>
conclusion: <supported|unsupported|unresolved|unknown, with scope>
confidence: <high|medium|low|unknown>
limits: [<unverified assumptions or conflicting scope>]
validation_needed: [<specific build, test, or additional source>]
persisted_as: <none|knowledge/android/decisions/...|knowledge/android/lessons/...>
```

Confidence describes evidence for the stated claim, not probability that code works: **high** means exact, current, directly applicable primary evidence with no material conflict; **medium** means direct evidence with a stated scope or validation gap; **low** means indirect evidence or unresolved assumptions and must not support a firm recommendation; **unknown** means necessary versions/evidence are absent or inaccessible.

## Worked examples

These scenarios are hypothetical and are not this repository’s version baseline. The cited moving official pages were checked on **2026-10-13 UTC**; repeat the checks before using the facts in an engineering decision.

### Compose Navigation 3 with adaptive UI

**Question:** “Can this app use Navigation 3 for a list-detail layout that adapts between compact and expanded windows?”

**Select:** A1 [Navigation 3 guide](https://developer.android.com/guide/navigation/navigation-3), [adaptive apps guide](https://developer.android.com/develop/ui/compose/build-adaptive-apps), [adaptive navigation guide](https://developer.android.com/develop/adaptive-apps/guides/build-adaptive-navigation), and the [Navigation 3 release record](https://developer.android.com/jetpack/androidx/releases/navigation3) if dependency status matters. Add the AndroidX release record for Material 3 adaptive navigation only if its artifact version is in question. A2 is unnecessary unless a behavior needs source-level explanation; A3 is unnecessary unless an example pattern is specifically requested.

**Verified snapshot:** The Navigation 3 guide describes a Compose navigation library and discusses `NavDisplay` scene strategies for adaptive layouts. The adaptive navigation guide identifies `NavigationSuiteScaffold` with the `androidx.compose.material3:material3-adaptive-navigation-suite` artifact. The Navigation 3 release page checked for this example listed 1.2.0 as stable and 1.3.0-alpha02 as alpha. These are dated upstream observations, not a recommendation to adopt either version.

**Conclusion:** The official guidance describes the design path. Whether a particular app’s dependency set supports it is **unknown** until its declared/resolved versions and required release channel are inspected and checked against current release records; a design guide or sample does not prove that build.

### Kotlin, AGP, Gradle, and JDK compatibility

**Question:** “Does this hypothetical tuple have a documented compatibility intersection?” Inputs: KGP 2.4.20, AGP 9.2.0, Gradle 9.7.0, and JDK 17 to run Gradle. These numbers are an example only, not a project baseline.

**Select:** A1 [Kotlin Gradle compatibility](https://kotlinlang.org/docs/gradle-configure-project.html), [AGP 9.2.0 release notes](https://developer.android.com/build/releases/agp-9-2-0-release-notes), [Gradle 9.7 compatibility matrix](https://docs.gradle.org/9.7.0/userguide/compatibility.html), and [Android JDK guidance](https://developer.android.com/build/jdks). Record which Java is the Gradle runtime separately from compile toolchains and bytecode targets.

**Verified snapshot:** On 2026-10-13, Kotlin’s table listed KGP 2.4.20 with fully supported Gradle 7.6.3–9.7.0 and AGP 8.5.2–9.3.1. AGP 9.2.0 listed Gradle 9.4.1 as minimum/default and JDK 17; Gradle 9.7’s matrix included Java 17 for running Gradle. The tuple is inside those documented ranges on that date.

**Conclusion:** Report `documented range intersection: yes, as checked 2026-10-13`; do not report `build verified`. Recheck exact release pages and plugin declarations. AGP/KGP runtime details, third-party plugins, build logic, and project tests can add constraints; if a release note’s runtime dependency is ambiguous, preserve that uncertainty. A successful build on the exact tuple is separate evidence.

### Material 3 Expressive and Compose BOM alignment

**Question:** “Will the project’s Compose BOM provide the Material 3 Expressive API we need, and is that API stable?” The hypothetical request does not name an API or BOM version.

**Select:** A1 [Material 3 in Compose](https://developer.android.com/develop/ui/compose/designsystems/material3), [BOM usage](https://developer.android.com/develop/ui/compose/bom), the [BOM-to-library mapping](https://developer.android.com/develop/ui/compose/bom/bom-mapping) for the exact BOM version, and [Material 3 release notes](https://developer.android.com/jetpack/androidx/releases/compose-material3) for the exact artifact/API status. Use A2 only for implementation details not answered by those pages.

**Verified snapshot:** The Material 3 guide describes Material 3 Expressive as part of the Compose Material 3 design system. The BOM guide says the selected BOM maps Compose library versions; it distinguishes stable, beta, and alpha BOMs, and says beta/alpha BOMs are for testing rather than production. It permits explicit per-library overrides. The Material 3 release record has separate channels and API-change notes. None of these facts identifies the unspecified requested API’s maturity or the project’s selected mapping.

**Conclusion:** `availability and stability: unknown` until the exact API, artifact, BOM coordinate/version, mapped Material 3 version, and API release/opt-in status are checked. Do not infer stable support from the “Expressive” design label or from using any BOM. If the required API is preview-only, state the channel and trade-off explicitly; do not silently substitute an alpha/beta BOM or override.

## Repository baseline and limits

At baseline commit `f2fe8273dcafd6409de65b5e9444592c3286af72`, explicit Avenox context retrieval returned the source registry and the decision/lesson templates, with no Android build baseline or project engineering decision. This repository therefore has no established Android/Kotlin/AGP/Gradle/Compose version tuple. Re-run the project-file inspection and Avenox query as the repository evolves. This protocol does not run builds, install tools, mirror upstream docs, or automate freshness checks.
