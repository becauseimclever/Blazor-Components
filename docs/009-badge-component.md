# Feature 009: Badge Component
> **Status:** Planning

## Overview

Extract the Badge component from BudgetExperiment. A small label component for status indicators, counts, or tags with color variants.

## Problem Statement

### Current State

Badges are used in BudgetExperiment for status labels and counts. This is a common UI primitive.

### Target State

A Badge component in the NuGet package.

---

## User Stories

### US-009-001: Badge Display
**As a** consuming developer  
**I want to** display a small colored label  
**So that** I can show status, counts, or tags.

**Acceptance Criteria:**
- [ ] Renders text content
- [ ] Supports color variants (default, success, warning, danger, info)
- [ ] Supports pill shape option

---

## Technical Design

### Component API

```razor
<Badge Variant="BadgeVariant.Success">Active</Badge>
<Badge Variant="BadgeVariant.Danger" Pill="true">3</Badge>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Variant | BadgeVariant | Default | Color variant |
| Pill | bool | false | Pill/rounded shape |
| ChildContent | RenderFragment | — | Badge text |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: Badge
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement Badge.razor + Badge.razor.css

**Commit:** `feat(badge): add Badge component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders ChildContent
- [ ] Each variant applies correct CSS class
- [ ] Pill adds pill class
- [ ] Custom CSS class applied

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Badge.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
