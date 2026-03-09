# Feature 012: ThemeToggle Component
> **Status:** Planning

## Overview

Extract the ThemeToggle component from BudgetExperiment. Provides a dropdown/button to switch between light, dark, and system themes using CSS custom properties.

## Problem Statement

### Current State

BudgetExperiment supports 10+ themes with a ThemeToggle component. The theming infrastructure (CSS variables) and theme switcher are reusable.

### Target State

A ThemeToggle component and supporting ThemeService in the NuGet package.

---

## User Stories

### US-012-001: Theme Switching
**As a** consuming developer  
**I want to** provide a theme toggle control  
**So that** users can switch between light, dark, and system themes.

**Acceptance Criteria:**
- [ ] Dropdown or toggle showing available themes
- [ ] Selected theme persisted (via callback or service)
- [ ] CSS custom properties updated on theme change

---

## Technical Design

### Component API

```razor
<ThemeToggle CurrentTheme="@_theme" OnThemeChanged="HandleThemeChanged" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| CurrentTheme | string | "system" | Current theme name |
| Themes | IReadOnlyList<string>? | null | Available themes (default: light, dark, system) |
| OnThemeChanged | EventCallback<string> | — | Theme change callback |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: ThemeToggle
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement ThemeToggle.razor + ThemeToggle.razor.css
- [ ] Define base CSS custom property contracts

**Commit:** `feat(theme): add ThemeToggle component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders current theme
- [ ] Dropdown shows available themes
- [ ] OnThemeChanged fires with selected theme
- [ ] Custom theme list renders correctly

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/ThemeToggle.razor`
- Related: BudgetExperiment THEMING.md docs

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
