# Copilot Instructions / Engineering Guide

Authoritative guidance for GitHub Copilot and contributors working on BecauseImClever.Components.

## 1. Solution Purpose
BecauseImClever.Components: A .NET 10 Razor Class Library (RCL) providing reusable Blazor UI components as a NuGet package. Components are extracted from the BecauseImClever and BudgetExperiment projects to enable sharing across multiple applications.

## 2. Architecture
Single Razor Class Library with supporting test project:
- `src/BecauseImClever.Components` — Razor Class Library (NuGet package)
- `tests/BecauseImClever.Components.Tests` — bUnit + xUnit tests

## 3. Technology Stack
- .NET 10
- Blazor (Razor Class Library) — plain Blazor components only, NO FluentUI-Blazor
- xUnit + bUnit (component tests) + Shouldly (assertions)
- MinVer (automatic versioning from Git tags)
- StyleCop.Analyzers (code style enforcement)

## 4. Naming & Conventions
- C# Style: PascalCase types/members (except local variables & private fields). Private fields: `_camelCase`.
- One top-level class/record/struct per file. File name must match the type name.
- Async methods end with `Async`.
- Interfaces: prefix `I`.
- Component parameters: use `[Parameter]` attribute, PascalCase names.
- CSS: Use CSS isolation (`.razor.css`) and CSS variables (`--color-*`) for theming support.

## 5. TDD Workflow (ALWAYS)
1. Write a failing bUnit test (RED).
2. Implement minimal component code to pass (GREEN).
3. Refactor with SOLID & Clean Code (REFACTOR).
4. Ensure all tests pass before committing.

## 6. Component Design Guidelines
- Components should be self-contained and reusable across different Blazor apps.
- Use `[Parameter]` for all configurable properties.
- Use `EventCallback` / `EventCallback<T>` for component events.
- Use `RenderFragment` / `RenderFragment<T>` for templated content.
- Prefer CSS isolation over global styles.
- Support theming via CSS custom properties.
- No external CSS framework dependencies (no Bootstrap, Tailwind, etc.).
- Keep components thin — extract complex logic into testable C# classes/services.

## 7. Testing Strategy
- bUnit for all components with meaningful logic (form validation, state management, conditional rendering, event handling).
- Pure display-only components may have minimal tests or be skipped with documented justification.
- Use Shouldly for assertions (NO FluentAssertions).
- Use NSubstitute for mocking if needed (NO Moq, NO AutoFixture).

## 8. Versioning
- MinVer automatically sets version from Git tags (prefix `v`, e.g., `v1.0.0`).
- Pre-release versions: `v1.0.0-preview.1`, `v1.0.0-beta.1`, `v1.0.0-rc.1`.
- Tag format: `v{major}.{minor}.{patch}[-prerelease]`.

## 9. NuGet Package Management
- Always add or update NuGet dependencies using the `dotnet` CLI.
- Keep versions explicit (no floating ranges).
- Do NOT hand-edit `<PackageReference>` blocks unless necessary.

## 10. Feature Documentation Lifecycle
- Each component extraction tracked as a numbered feature doc in `docs/`.
- Use `docs/FEATURE-TEMPLATE.md` as the starting template.
- Set `Status` to `In Progress` when starting, `Done` when complete.
- Check off completed acceptance criteria and implementation tasks.

## 11. Git / Branching
- Main: stable.
- Feature branches: `feature/<short-desc>`.
- Always include tests in same PR; failing tests block merge.

## 12. Conventional Commits
Use conventional commits for proper changelog generation:

| Type | When to Use | Example |
|------|-------------|---------|
| `feat` | New component or capability | `feat(button): add Button component` |
| `fix` | Bug fix | `fix(modal): correct focus trap` |
| `docs` | Documentation only | `docs: update component catalog` |
| `test` | Adding or fixing tests | `test(icon): add Icon rendering tests` |
| `chore` | Build, CI, dependencies | `chore: update NuGet packages` |
| `refactor` | Code restructure | `refactor(theming): extract CSS variable system` |

## 13. Shell / Scripting Guidelines
- PowerShell commands must use fully qualified paths.
- Avoid stateful assumptions about current directory.

## 14. Forbidden / Discouraged
- FluentUI-Blazor
- FluentAssertions
- AutoFixture
- External CSS frameworks (Bootstrap, Tailwind, etc.)
- God components (> ~200 lines of code-behind)

## 15. Style / Analyzers
- Enable nullable reference types.
- Treat warnings as errors.
- StyleCop enforced via `Directory.Build.props`.
- Root `stylecop.json` + `.editorconfig` define rule set.

---
Keep this file lean—prune when obsolete. Update when architectural decisions shift.
