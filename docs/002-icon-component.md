# Feature 002: Icon Component
> **Status:** Planning

## Overview

Extract the Icon component from BudgetExperiment. The Icon component renders inline SVG icons from a library of 200+ Lucide icons, supporting size, color, and accessibility attributes. This is a foundational component used by many others (Button, NavMenu, PageHeader, etc.).

## Problem Statement

### Current State

The Icon component with 200+ SVG paths exists in BudgetExperiment. Other BecauseImClever projects duplicate icon rendering or lack a consistent icon system.

### Target State

A standalone Icon component in the NuGet package with a curated set of commonly-used icons, extensible for adding more.

---

## User Stories

### US-002-001: Render Icons by Name
**As a** consuming developer  
**I want to** render an icon by specifying its name  
**So that** I can use consistent icons throughout my app.

**Acceptance Criteria:**
- [ ] Icon renders correct SVG path for given name
- [ ] Unknown icon name renders nothing or fallback

### US-002-002: Icon Sizing
**As a** consuming developer  
**I want to** control icon size  
**So that** icons fit different contexts (inline text, buttons, headers).

**Acceptance Criteria:**
- [ ] Supports size parameter (small, medium, large, or pixel value)
- [ ] Default size is appropriate for inline use

### US-002-003: Accessibility
**As a** consuming developer  
**I want** icons to be accessible  
**So that** screen readers handle decorative vs. informative icons correctly.

**Acceptance Criteria:**
- [ ] Decorative icons have `aria-hidden="true"`
- [ ] Informative icons support `aria-label`

---

## Technical Design

### Component API

```razor
<Icon Name="IconName.Check" Size="IconSize.Medium" />
<Icon Name="IconName.Warning" AriaLabel="Warning" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Name | IconName | — | Icon identifier enum |
| Size | IconSize | Medium | Icon size |
| Color | string? | null | CSS color override |
| AriaLabel | string? | null | Accessible label (if set, icon is informative) |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: Core Icon with Initial Set
**Tasks:**
- [ ] Create IconName enum with initial set (~30 most common icons)
- [ ] Write bUnit tests for rendering, sizes, accessibility
- [ ] Implement Icon.razor + SVG path dictionary

**Commit:** `feat(icon): add Icon component with initial icon set`

### Phase 2: Extended Icon Set
**Tasks:**
- [ ] Add remaining commonly-used icons from BudgetExperiment
- [ ] Tests for new icons

**Commit:** `feat(icon): expand icon library`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders SVG element with correct viewBox
- [ ] Renders correct path for known icon names
- [ ] Size parameter controls width/height
- [ ] Decorative icons have aria-hidden
- [ ] AriaLabel sets aria-label and removes aria-hidden
- [ ] Unknown icon renders gracefully

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Icon.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
