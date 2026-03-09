# Feature 014: ProgressBar Component
> **Status:** Planning

## Overview

Extract the ProgressBar component from BudgetExperiment. Displays a horizontal progress bar with percentage, color variants, and optional label.

## Problem Statement

### Current State

Progress bars are used in BudgetExperiment for budget tracking and import progress.

### Target State

A ProgressBar component in the NuGet package.

---

## User Stories

### US-014-001: Progress Display
**As a** consuming developer  
**I want to** show progress as a horizontal bar  
**So that** users can see completion status.

**Acceptance Criteria:**
- [ ] Renders progress bar with percentage width
- [ ] Supports color variants
- [ ] Optional label text
- [ ] Supports indeterminate mode (animated, no percentage)

---

## Technical Design

### Component API

```razor
<ProgressBar Value="75" Max="100" Variant="ProgressVariant.Success" Label="75%" />
<ProgressBar Indeterminate="true" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Value | double | 0 | Current value |
| Max | double | 100 | Maximum value |
| Variant | ProgressVariant | Default | Color variant |
| Label | string? | null | Label text |
| Indeterminate | bool | false | Animated indeterminate mode |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: ProgressBar
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement ProgressBar.razor + ProgressBar.razor.css

**Commit:** `feat(progress): add ProgressBar component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders bar at correct percentage width
- [ ] Label renders when provided
- [ ] Color variant applies correct CSS class
- [ ] Indeterminate mode renders animation class
- [ ] Value clamped between 0 and Max

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Charts/ProgressBar.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
