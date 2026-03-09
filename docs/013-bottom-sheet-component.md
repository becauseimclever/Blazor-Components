# Feature 013: BottomSheet Component
> **Status:** Planning

## Overview

Extract the BottomSheet component from BudgetExperiment. A mobile-friendly slide-up panel for actions or content, commonly used for mobile navigation and action menus.

## Problem Statement

### Current State

BudgetExperiment uses a BottomSheet for mobile-optimized UI interactions.

### Target State

A BottomSheet component in the NuGet package.

---

## User Stories

### US-013-001: Bottom Sheet Panel
**As a** consuming developer  
**I want to** show a slide-up panel from the bottom of the screen  
**So that** I can provide mobile-friendly interaction patterns.

**Acceptance Criteria:**
- [ ] Slides up from bottom with animation
- [ ] Backdrop overlay
- [ ] Close on backdrop click
- [ ] Close on swipe down (optional)
- [ ] ChildContent for panel body

---

## Technical Design

### Component API

```razor
<BottomSheet IsVisible="@_showSheet" OnClose="HandleClose" Title="Actions">
    <p>Bottom sheet content</p>
</BottomSheet>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| IsVisible | bool | false | Controls visibility |
| Title | string? | null | Panel title |
| OnClose | EventCallback | — | Close callback |
| CloseOnBackdropClick | bool | true | Close when clicking backdrop |
| ChildContent | RenderFragment | — | Panel content |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: BottomSheet
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement BottomSheet.razor + BottomSheet.razor.css

**Commit:** `feat(bottom-sheet): add BottomSheet component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders when IsVisible=true
- [ ] Hidden when IsVisible=false
- [ ] Title renders
- [ ] ChildContent renders
- [ ] OnClose fires on backdrop click

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/BottomSheet.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
