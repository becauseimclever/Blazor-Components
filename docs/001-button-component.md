# Feature 001: Button Component
> **Status:** Planning

## Overview

Extract the reusable Button component from BudgetExperiment. The Button component provides a consistent, accessible button with support for variants (primary, secondary, danger, ghost), sizes, loading states, disabled states, and icon slots.

## Problem Statement

### Current State

The Button component exists in the BudgetExperiment Client project. It is tightly coupled to that application and cannot be reused in other BecauseImClever projects (e.g., the main becauseimclever app).

### Target State

A standalone, well-tested Button component in the BecauseImClever.Components NuGet package that any Blazor app can consume.

---

## User Stories

### US-001-001: Render Button Variants
**As a** consuming developer  
**I want to** specify a button variant (primary, secondary, danger, ghost)  
**So that** I can match the button style to its action context.

**Acceptance Criteria:**
- [ ] Button renders with correct CSS class per variant
- [ ] Default variant is "primary"

### US-001-002: Button Sizes
**As a** consuming developer  
**I want to** specify button size (small, medium, large)  
**So that** buttons fit different layout contexts.

**Acceptance Criteria:**
- [ ] Button renders correct size class
- [ ] Default size is "medium"

### US-001-003: Loading & Disabled States
**As a** consuming developer  
**I want to** show a loading spinner and disable the button during async operations  
**So that** users get visual feedback and can't double-click.

**Acceptance Criteria:**
- [ ] Loading state shows spinner and disables button
- [ ] Disabled state prevents click events

### US-001-004: Icon Support
**As a** consuming developer  
**I want to** add an icon before or after the button text  
**So that** buttons have clear visual affordances.

**Acceptance Criteria:**
- [ ] Supports ChildContent for main label
- [ ] Supports optional icon RenderFragment

---

## Technical Design

### Component API

```razor
<Button Variant="ButtonVariant.Primary"
        Size="ButtonSize.Medium"
        Loading="@_isSaving"
        Disabled="@_isDisabled"
        OnClick="HandleClick">
    Save Changes
</Button>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Variant | ButtonVariant | Primary | Visual style |
| Size | ButtonSize | Medium | Button size |
| Loading | bool | false | Shows loading spinner |
| Disabled | bool | false | Disables interaction |
| Type | string | "button" | HTML button type |
| OnClick | EventCallback<MouseEventArgs> | — | Click handler |
| ChildContent | RenderFragment | — | Button label |
| CssClass | string? | null | Additional CSS classes |

### CSS / Theming

CSS isolation with CSS custom properties for color theming:
- `--btn-bg`, `--btn-color`, `--btn-border`
- `--btn-bg-hover`, `--btn-color-hover`

---

## Implementation Plan

### Phase 1: Core Button

**Tasks:**
- [ ] Create ButtonVariant and ButtonSize enums
- [ ] Write bUnit tests for rendering, variants, sizes
- [ ] Implement Button.razor + Button.razor.css

**Commit:** `feat(button): add Button component with variants and sizes`

### Phase 2: Loading & Disabled States

**Tasks:**
- [ ] Write bUnit tests for loading/disabled behavior
- [ ] Implement loading spinner and disabled logic

**Commit:** `feat(button): add loading and disabled states`

### Phase 3: Documentation

**Tasks:**
- [ ] Add XML comments
- [ ] Update README component catalog

**Commit:** `docs(button): add Button component documentation`

---

## Testing Strategy

### bUnit Tests

- [ ] Renders with default parameters
- [ ] Renders each variant with correct CSS class
- [ ] Renders each size with correct CSS class
- [ ] Click event fires when not disabled/loading
- [ ] Click event does NOT fire when disabled
- [ ] Click event does NOT fire when loading
- [ ] Loading state shows spinner
- [ ] Custom CSS class is applied
- [ ] ChildContent renders correctly

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Button.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
