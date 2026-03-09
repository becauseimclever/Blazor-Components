# BecauseImClever.Components

A .NET 10 Razor Class Library (RCL) providing reusable Blazor UI components as a NuGet package. Components are extracted from the [BecauseImClever](https://github.com/becauseimclever/becauseimclever) and [BudgetExperiment](https://github.com/becauseimclever/BudgetExperiment) projects.

## Installation

```bash
dotnet add package BecauseImClever.Components
```

## Usage

Add the namespace to your `_Imports.razor`:

```razor
@using BecauseImClever.Components
```

## Component Catalog

| # | Component | Status | Description |
|---|-----------|--------|-------------|
| 001 | Button | Planning | Buttons with variants, sizes, loading states |
| 002 | Icon | Planning | SVG icon library (Lucide-based) |
| 003 | Modal | Planning | Accessible dialog overlay |
| 004 | ConfirmDialog | Planning | Confirm/cancel dialog |
| 005 | LoadingSpinner | Planning | Loading indicator with sizes |
| 006 | ErrorAlert | Planning | Error display with retry/dismiss |
| 007 | PageHeader | Planning | Page title bar with actions |
| 008 | Card | Planning | Content container card |
| 009 | Badge | Planning | Status/count label |
| 010 | EmptyState | Planning | Empty data placeholder |
| 011 | FormField | Planning | Form field wrapper with validation |
| 012 | ThemeToggle | Planning | Theme switcher (light/dark/system) |
| 013 | BottomSheet | Planning | Mobile slide-up panel |
| 014 | ProgressBar | Planning | Progress indicator |
| 015 | MarkdownEditor | Planning | Markdown editor with preview |
| 016 | ConsentBanner | Planning | GDPR consent banner |

See `docs/` for detailed feature specifications per component.

## Development

### Prerequisites

- .NET 10 SDK

### Build

```bash
dotnet build BecauseImClever.Components.sln
```

### Test

```bash
dotnet test BecauseImClever.Components.sln
```

### Pack

```bash
dotnet pack src/BecauseImClever.Components/BecauseImClever.Components.csproj --configuration Release
```

## Versioning

Uses [MinVer](https://github.com/adamralph/minver) for automatic versioning from Git tags.

- Tag format: `v{major}.{minor}.{patch}` (e.g., `v1.0.0`)
- Pre-release: `v1.0.0-preview.1`, `v1.0.0-beta.1`

## CI/CD

- **CI**: Build + test on push/PR to `main` (`.github/workflows/ci.yml`)
- **NuGet Publish**: Pack + push on version tags (`.github/workflows/nuget-publish.yml`)
- **Release**: GitHub Release with changelog on version tags (`.github/workflows/release.yml`)

## License

[MIT](LICENSE)
A collection of reusable blazor components.
