# Feature 002: Icon Component
> **Status:** Planning

## Overview

Build an Icon component backed by a **library-agnostic pre-build code generation tool** that can process any SVG icon library. The first supported library is the **complete Lucide icon set** (~1,500+ icons). A `dotnet tool` reads SVG files from git submodules and generates checked-in C# code — a typed enum + SVG data dictionary per library — giving consuming developers type-safe access to every icon with zero runtime cost and fast CI builds.

A **GitHub Action** automatically detects new releases of icon submodules, runs the generator, and opens a PR with the updated code.

The architecture is designed so that additional SVG icon libraries (e.g., Bootstrap Icons, Heroicons, Tabler Icons) can be added in the future by adding a submodule and a configuration entry — no tool code changes required.

The BudgetExperiment Icon component manually inlines ~90 SVG paths in a switch expression. This approach replaces that with an automated, maintainable solution.

## Problem Statement

### Current State

BudgetExperiment's Icon component hard-codes ~90 Lucide icon SVG paths in a giant switch expression. Adding a new icon requires manually copying SVG markup. The subset is arbitrary and frequently insufficient, forcing developers to add icons ad-hoc.

### Target State

- **Complete Lucide coverage**: Every icon from [lucide-icons/lucide](https://github.com/lucide-icons/lucide) available via a generated `LucideIcon` enum.
- **Pre-build code generation tool**: A `dotnet tool` reads SVG files from submodules and generates checked-in `.cs` files (per-library enum + data class). No generation happens at compile time — CI builds are fast.
- **Git submodule for source SVGs**: Lucide SVGs are pulled via a git submodule pinned to a release tag.
- **Automated icon updates**: A GitHub Action detects new submodule releases, runs the generator, and opens a PR with updated code.
- **Type-safe**: `LucideIcon.ArrowRight` instead of magic strings like `"arrow-right"`.
- **Extensible**: Future icon libraries produce their own enum (e.g., `BootstrapIcon.Check`) and data class, following the same pattern.

---

## User Stories

### US-002-001: Render Icons by Name (Type-Safe)
**As a** consuming developer
**I want to** render an icon using a strongly-typed enum value
**So that** I get compile-time safety and IDE autocomplete for all available icons.

**Acceptance Criteria:**
- [ ] `LucideIcon` enum contains an entry for every Lucide icon
- [ ] Icon renders the correct SVG markup for the given enum value
- [ ] Enum values use PascalCase (e.g., `LucideIcon.ArrowRight` for `arrow-right.svg`)

### US-002-002: Icon Sizing
**As a** consuming developer
**I want to** control icon size via a parameter
**So that** icons fit different contexts (inline text, buttons, headers).

**Acceptance Criteria:**
- [ ] Supports `Size` parameter (int, in pixels)
- [ ] Default size is 24 (Lucide default)
- [ ] Size sets both `width` and `height` attributes on the SVG

### US-002-003: Accessibility
**As a** consuming developer
**I want** icons to be accessible
**So that** screen readers handle decorative vs. informative icons correctly.

**Acceptance Criteria:**
- [ ] Decorative icons (no Title) have `aria-hidden="true"` and no `role`
- [ ] Informative icons (Title set) have `role="img"`, `aria-label`, and a `<title>` element
- [ ] `focusable="false"` is always set to prevent IE/Edge focus issues

### US-002-004: Stroke Customization
**As a** consuming developer
**I want to** control stroke width
**So that** I can match icon weight to my app's typography.

**Acceptance Criteria:**
- [ ] `StrokeWidth` parameter (double, default 2)
- [ ] Applied to the SVG `stroke-width` attribute

---

## Technical Design

### Solution Architecture

```
src/
  BecauseImClever.Components/                    # Razor Class Library (existing)
    Components/
      Icon.razor                                  # The Icon component
      Icon.razor.cs                               # Code-behind
      Icon.razor.css                              # CSS isolation
    Generated/                                    # Checked-in generated code
      IIconName.cs                                # Marker interface
      IconRegistry.cs                             # Multi-library dispatcher
      Lucide/
        LucideIcon.cs                             # Generated enum (~1,500 members)
        LucideIconData.cs                         # Generated FrozenDictionary
tools/
  BecauseImClever.IconGenerator/                  # dotnet tool (console app, NEW)
    BecauseImClever.IconGenerator.csproj
    Program.cs                                    # CLI entry point
    SvgParser.cs                                  # SVG element extraction
    IconNameConverter.cs                          # kebab-case → PascalCase + C# identifier rules
    CodeEmitter.cs                                # C# code file writer
    icon-libraries.json                           # Library configuration (name, submodule path, SVG glob, defaults)
submodules/
  lucide/                                         # Git submodule → github.com/lucide-icons/lucide
    icons/                                        # SVG source files (~1,500+)
      arrow-right.svg
      check.svg
      ...
tests/
  BecauseImClever.Components.Tests/               # Component tests (existing)
  BecauseImClever.IconGenerator.Tests/            # Generator tool unit tests (NEW)
.github/
  workflows/
    update-icons.yml                              # Automated icon update workflow (NEW)
.gitattributes                                    # Marks Generated/ as linguist-generated
```

### Git Submodule Setup

Lucide SVGs are managed as a git submodule pinned to a specific release tag:

```bash
git submodule add https://github.com/lucide-icons/lucide.git submodules/lucide
cd submodules/lucide
git checkout v0.460.0   # pin to a release tag
cd ../..
git add .gitmodules submodules/lucide
git commit -m "chore: add lucide icons submodule at v0.460.0"
```

**CI consideration:** Workflows that build the project do NOT need `submodules: true` because generated code is checked in. Only the `update-icons.yml` workflow needs submodules.

### Pre-build Code Generation Tool

**Project:** `tools/BecauseImClever.IconGenerator` — a `dotnet tool` (console app targeting `net10.0`)

Unlike a source generator, this tool runs **on demand** (locally or in CI), produces `.cs` files that are **checked into source control**, and is not involved at compile time. This means:
- CI builds are identical to any normal .NET build — no SVG parsing overhead
- Generated code is visible in PRs and diffs
- Easy to debug and test — it's just a console app
- No `netstandard2.0` limitation

#### Library Configuration (`icon-libraries.json`)

```json
[
  {
    "name": "Lucide",
    "submodulePath": "submodules/lucide",
    "svgGlob": "icons/*.svg",
    "namespace": "BecauseImClever.Components.Icons",
    "defaults": {
      "viewBox": "0 0 24 24",
      "fill": "none",
      "stroke": "currentColor",
      "strokeWidth": 2,
      "strokeLinecap": "round",
      "strokeLinejoin": "round"
    }
  }
]
```

Adding a new library is just a new entry in this JSON file + a new submodule.

#### CLI Usage

```bash
# Generate all icon libraries
dotnet run --project tools/BecauseImClever.IconGenerator

# Verify generated code matches SVGs (for CI checks)
dotnet run --project tools/BecauseImClever.IconGenerator -- --verify

# Generate a specific library only
dotnet run --project tools/BecauseImClever.IconGenerator -- --library Lucide
```

The `--verify` flag exits with a non-zero code if the checked-in files don't match what the tool would generate. This can be used as a CI gate to ensure generated code stays in sync.

#### Generated Output (per library)

1. **`IIconName.cs`** — shared marker interface:
```csharp
namespace BecauseImClever.Components.Icons;

public interface IIconName
{
    string Name { get; }
    string SvgMarkup { get; }
}
```

2. **`{Library}Icon.cs`** — enum with one member per SVG file, PascalCase:
```csharp
namespace BecauseImClever.Components.Icons;

public enum LucideIcon
{
    ArrowRight,
    Check,
    ChevronDown,
    // ... ~1,500+ entries
}
```

3. **`{Library}IconData.cs`** — frozen dictionary of enum → SVG inner markup:
```csharp
namespace BecauseImClever.Components.Icons;

internal static class LucideIconData
{
    internal static readonly FrozenDictionary<LucideIcon, string> Paths =
        new Dictionary<LucideIcon, string>
        {
            [LucideIcon.ArrowRight] = "<path d=\"M5 12h14\"/><path d=\"m12 5 7 7-7 7\"/>",
            [LucideIcon.Check] = "<path d=\"M20 6 9 17l-5-5\"/>",
            // ...
        }.ToFrozenDictionary();
}
```

4. **`IconRegistry.cs`** — multi-library dispatcher (regenerated when libraries change):
```csharp
namespace BecauseImClever.Components.Icons;

internal static class IconRegistry
{
    internal static string? GetSvgMarkup(LucideIcon icon) =>
        LucideIconData.Paths.GetValueOrDefault(icon);

    // Future: overload per library
    // internal static string? GetSvgMarkup(BootstrapIcon icon) =>
    //     BootstrapIconData.Paths.GetValueOrDefault(icon);
}
```

**Name conversion:** `kebab-case` file name → `PascalCase` enum member (e.g., `arrow-up-right.svg` → `ArrowUpRight`). Numeric prefixes get an underscore prefix to satisfy C# identifier rules (e.g., `2-columns.svg` → `_2Columns`).

#### Source Control for Generated Files

Generated files are checked in but marked in `.gitattributes`:
```gitattributes
src/BecauseImClever.Components/Generated/** linguist-generated=true
```

This collapses them in GitHub PR diffs by default while keeping them available for review when needed.

### GitHub Action: Automated Icon Updates (`update-icons.yml`)

A scheduled workflow checks for new releases of each icon submodule, and if updates are found, runs the generator and opens a PR.

```yaml
name: Update Icons

on:
  schedule:
    - cron: '0 6 * * 1'  # Weekly on Monday at 6am UTC
  workflow_dispatch:       # Manual trigger
    inputs:
      library:
        description: 'Icon library to update (blank = all)'
        required: false
        type: string

jobs:
  update-icons:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '10.0.x'

      - name: Check for submodule updates
        id: check
        run: |
          updated=false
          # For each submodule, check if there's a newer release tag
          git submodule foreach --quiet '
            cd $toplevel/$sm_path
            git fetch --tags
            current=$(git describe --tags --exact-match 2>/dev/null || git rev-parse --short HEAD)
            latest=$(git tag -l "v*" --sort=-v:refname | head -1)
            if [ "$current" != "$latest" ] && [ -n "$latest" ]; then
              echo "::notice::$sm_path: $current → $latest"
              git checkout "$latest"
              echo "updated=true" >> $GITHUB_OUTPUT
              echo "$sm_path=$latest" >> $GITHUB_OUTPUT
            else
              echo "::notice::$sm_path: up to date at $current"
            fi
          '

      - name: Run icon generator
        if: steps.check.outputs.updated == 'true'
        run: dotnet run --project tools/BecauseImClever.IconGenerator

      - name: Build and test
        if: steps.check.outputs.updated == 'true'
        run: |
          dotnet build --configuration Release
          dotnet test --configuration Release --no-build

      - name: Create Pull Request
        if: steps.check.outputs.updated == 'true'
        uses: peter-evans/create-pull-request@v6
        with:
          commit-message: 'chore(icon): update icon libraries'
          title: 'chore(icon): update icon libraries'
          body: |
            Automated icon library update.

            This PR was created by the `update-icons` workflow.
            The submodule(s) were updated to the latest release tag
            and the icon code was regenerated.

            Please review the generated code changes.
          branch: chore/update-icons
          labels: dependencies, automated
```

**Trigger mechanisms:**
- **Scheduled (weekly):** Runs every Monday at 6am UTC, checks all submodules for new release tags
- **Manual (`workflow_dispatch`):** Trigger on demand from the Actions tab, optionally targeting a specific library
- **Automatic on submodule push:** If Dependabot or another workflow updates the submodule pointer, the CI workflow's `--verify` check will fail, signaling that the generator needs to run. Alternatively, a `push` trigger filtered to `.gitmodules` or `submodules/**` paths can auto-trigger:

```yaml
on:
  push:
    paths:
      - '.gitmodules'
      - 'submodules/**'
    branches: [master]
```

**Safety:** The workflow builds and runs tests after regeneration. The PR is only created if the build and tests pass. The PR requires normal review and merge — nothing is auto-merged.

### Component API

```razor
@* Lucide icons (first supported library) *@
<Icon Name="LucideIcon.Check" />
<Icon Name="LucideIcon.AlertTriangle" Size="32" StrokeWidth="1.5" />
<Icon Name="LucideIcon.Info" Title="Information" Class="text-blue" />

@* Future: other libraries use the same component, different enum *@
@* <Icon Name="BootstrapIcon.Check" /> *@
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Name | Enum (via overloads or generic) | — (required) | Icon to render (any generated icon enum) |
| Size | int | 24 | Width and height in pixels |
| StrokeWidth | double | 2 | SVG stroke width |
| Title | string? | null | Accessible title (makes icon informative) |
| Class | string? | null | Additional CSS classes |

### SVG Rendering

The component renders a standard SVG wrapper with the generated inner markup. Wrapper attributes come from the library defaults in `icon-libraries.json`:
```html
<svg xmlns="http://www.w3.org/2000/svg"
     width="24" height="24"
     viewBox="0 0 24 24"
     fill="none"
     stroke="currentColor"
     stroke-width="2"
     stroke-linecap="round"
     stroke-linejoin="round"
     class="lucide lucide-check"
     aria-hidden="true"
     focusable="false">
  <path d="M20 6 9 17l-5-5"/>
</svg>
```

### CSS Isolation

```css
:host {
    display: inline-flex;
    align-items: center;
    vertical-align: middle;
}
```

---

## Implementation Plan

### Phase 1: Submodule & Code Generation Tool
**Tasks:**
- [ ] Add `lucide-icons/lucide` as git submodule at `submodules/lucide`, pinned to a release tag
- [ ] Create `tools/BecauseImClever.IconGenerator` console app (net10.0)
- [ ] Create `icon-libraries.json` configuration for Lucide
- [ ] Implement `SvgParser` to extract inner elements from SVG files (library-agnostic)
- [ ] Implement `IconNameConverter` for kebab-case → PascalCase conversion
- [ ] Implement `CodeEmitter` to write per-library `.cs` files (enum, data, registry, interface)
- [ ] Implement `--verify` flag for CI freshness checks
- [ ] Add `.gitattributes` marking `Generated/` as `linguist-generated`
- [ ] Run the tool and check in the initial generated code for Lucide
- [ ] Add tool unit tests (SVG parsing, name conversion, code emission, verify mode)

**Commit:** `feat(icon): add icon code generation tool with Lucide submodule`

### Phase 2: Icon Component
**Tasks:**
- [ ] Write bUnit tests for Icon rendering, sizing, accessibility, stroke width
- [ ] Implement `Icon.razor` / `Icon.razor.cs` using `IconRegistry`
- [ ] Add CSS isolation (`Icon.razor.css`)
- [ ] Verify generated `LucideIcon` enum has all expected icons (~1,500+)

**Commit:** `feat(icon): add Icon component with full Lucide icon set`

### Phase 3: CI & Automated Updates
**Tasks:**
- [ ] Add `update-icons.yml` GitHub Action (scheduled + manual + submodule push triggers)
- [ ] Add `--verify` step to the existing CI workflow to ensure generated code is fresh
- [ ] Verify CI build has no SVG parsing overhead (standard .NET build time)
- [ ] Test the full update flow: bump submodule → run tool → PR created → build passes

**Commit:** `chore(icon): add automated icon update workflow`

### Phase 4: Packaging
**Tasks:**
- [ ] Verify NuGet package contains the generated code (enum, data, component) but NOT the tool or raw SVGs
- [ ] Verify consuming projects get type-safe icon enums and the Icon component

**Commit:** `chore(icon): verify NuGet packaging`

---

## Testing Strategy

### Tool Tests (`BecauseImClever.IconGenerator.Tests`)
- [ ] SVG parser extracts inner elements correctly (path, circle, line, polyline, rect, ellipse)
- [ ] SVG parser strips outer `<svg>` wrapper attributes
- [ ] Name converter transforms kebab-case to PascalCase correctly
- [ ] Name converter handles edge cases (numeric prefix `2-columns` → `_2Columns`)
- [ ] Name converter handles single-word names (`check` → `Check`)
- [ ] Code emitter produces valid enum with expected member count for a test SVG set
- [ ] Code emitter produces valid data class with matching entries
- [ ] Tool processes multiple libraries from config into separate output files
- [ ] Tool produces `IIconName` interface and `IconRegistry`
- [ ] `--verify` flag returns success when code is fresh
- [ ] `--verify` flag returns failure when code is stale
- [ ] Tool handles empty/malformed SVG files gracefully (skip with warning)

### Component Tests (`BecauseImClever.Components.Tests`)
- [ ] Renders `<svg>` element with correct `viewBox="0 0 24 24"`
- [ ] Renders correct inner SVG markup for a known icon
- [ ] `Size` parameter sets `width` and `height` attributes
- [ ] Default size is 24
- [ ] `StrokeWidth` parameter sets `stroke-width` attribute
- [ ] Default stroke-width is 2
- [ ] Decorative icon (no Title) has `aria-hidden="true"` and `focusable="false"`
- [ ] Informative icon (Title set) has `role="img"`, `aria-label`, and `<title>` child element
- [ ] `Class` parameter is applied to the SVG element
- [ ] CSS class includes `lucide lucide-{kebab-name}`

---

## Key Decisions

| Decision | Rationale |
|----------|----------|
| Pre-build tool over source generator | Faster CI builds (no SVG parsing at compile time), debuggable, visible in PRs, no `netstandard2.0` constraint |
| Generated code checked in | CI builds are identical to any .NET project; generated code is reviewable; no build-time generation |
| `linguist-generated` in `.gitattributes` | Collapses generated diffs in GitHub PRs by default |
| `--verify` CI gate | Ensures generated code stays in sync with SVGs; fails CI if someone updates submodule without regenerating |
| Library-agnostic tool with `icon-libraries.json` | Adding a new icon library requires only a config entry + submodule — no tool code changes |
| Automated `update-icons.yml` workflow | Weekly check + manual trigger + submodule push trigger; runs tool, builds, tests, opens PR |
| PR-based updates (not auto-merge) | Safety — human reviews generated changes before they land on master |
| `FrozenDictionary` for icon data | Optimized for read-heavy, write-never lookup at runtime |
| Per-library typed enum (not strings) | Compile-time safety, autocomplete, prevents typos |
| SVG inner markup stored (not full SVG) | Component controls the `<svg>` wrapper (size, stroke, accessibility); only inner shapes vary per icon |
| Git submodule for icon sources | Pinned to a specific release tag; easy to update; works for any SVG icon repo |
| `submodules/` directory | Keeps icon source repos organized and separate from `src/`; scales to multiple submodules |
| `tools/` directory for the generator | Separates build tooling from library source; not included in NuGet package |

---

## Future: Adding a New Icon Library

When the need arises to add a second icon library (e.g., Bootstrap Icons):

1. **Add submodule:**
   ```bash
   git submodule add https://github.com/twbs/icons.git submodules/bootstrap-icons
   cd submodules/bootstrap-icons && git checkout v1.11.0 && cd ../..
   ```
2. **Add config entry** in `icon-libraries.json`:
   ```json
   {
     "name": "Bootstrap",
     "submodulePath": "submodules/bootstrap-icons",
     "svgGlob": "icons/*.svg",
     "namespace": "BecauseImClever.Components.Icons",
     "defaults": {
       "viewBox": "0 0 16 16",
       "fill": "currentColor"
     }
   }
   ```
3. **Run the tool:** `dotnet run --project tools/BecauseImClever.IconGenerator`
4. **Commit** the new generated files.
5. **Use:** `<Icon Name="BootstrapIcon.Check" />`

No changes to the generator tool or Icon component code are needed. The `update-icons.yml` workflow will automatically check for new releases of all submodules.

---

## References

- Lucide Icons: https://github.com/lucide-icons/lucide (ISC License)
- Lucide SVG format: 24×24 viewBox, stroke-based, standard element set
- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Icon.razor` (~90 icons, manual switch)
- `peter-evans/create-pull-request`: https://github.com/peter-evans/create-pull-request
- Potential future libraries: [Bootstrap Icons](https://github.com/twbs/icons), [Heroicons](https://github.com/tailwindlabs/heroicons), [Tabler Icons](https://github.com/tabler/tabler-icons)

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
| 2026-03-09 | Redesigned: source generator for full Lucide icon set | @copilot |
| 2026-03-09 | Updated: library-agnostic generator, git submodule approach, multi-library extensibility | @copilot |
| 2026-03-09 | Switched to pre-build tool approach with checked-in generated code for fast CI | @copilot |
| 2026-03-09 | Added automated icon update GitHub Action (scheduled + manual + submodule push triggers) | @copilot |
