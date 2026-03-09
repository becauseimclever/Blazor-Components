# Feature 005: LoadingSpinner Component
> **Status:** Planning

## Overview

Extract the LoadingSpinner component from BudgetExperiment. Provides a visual loading indicator with configurable size, message text, and optional full-page overlay mode.

## Problem Statement

### Current State

Loading indicators are needed across all Blazor apps. BudgetExperiment has a LoadingSpinner supporting multiple sizes and full-page mode.

### Target State

A standalone LoadingSpinner in the NuGet package.

---

## User Stories

### US-005-001: Show Loading State
**As a** consuming developer  
**I want to** display a spinner during async operations  
**So that** users know content is loading.

**Acceptance Criteria:**
- [ ] Renders spinner animation
- [ ] Supports size variants (small, medium, large)
- [ ] Supports optional message text
- [ ] Supports full-page overlay mode

---

## Technical Design

### Component API

```razor
<LoadingSpinner />
<LoadingSpinner Size="SpinnerSize.Large" Message="Loading data..." />
<LoadingSpinner FullPage="true" Message="Please wait..." />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Size | SpinnerSize | Medium | Spinner size |
| Message | string? | null | Loading message |
| FullPage | bool | false | Full-page overlay mode |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: LoadingSpinner
**Tasks:**
- [ ] Write bUnit tests for sizes, message, full-page mode
- [ ] Implement LoadingSpinner.razor + LoadingSpinner.razor.css

**Commit:** `feat(spinner): add LoadingSpinner component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders spinner element
- [ ] Size parameter sets correct CSS class
- [ ] Message text renders when provided
- [ ] Full-page mode adds overlay wrapper
- [ ] Custom CSS class applied

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/LoadingSpinner.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
