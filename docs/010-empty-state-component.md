# Feature 010: EmptyState Component
> **Status:** Planning

## Overview

Extract the EmptyState component from BudgetExperiment. Displays a centered placeholder with icon, title, description, and optional call-to-action when a list or section has no data.

## Problem Statement

### Current State

Empty state displays are common across pages. BudgetExperiment has a consistent EmptyState component.

### Target State

An EmptyState component in the NuGet package.

---

## User Stories

### US-010-001: Empty State Display
**As a** consuming developer  
**I want to** show a friendly empty state placeholder  
**So that** users know why a section is empty and what to do.

**Acceptance Criteria:**
- [ ] Renders icon, title, and description
- [ ] Optional action button/RenderFragment
- [ ] Centered layout

---

## Technical Design

### Component API

```razor
<EmptyState Icon="IconName.Inbox"
            Title="No transactions"
            Description="Add your first transaction to get started.">
    <Action>
        <Button OnClick="AddTransaction">Add Transaction</Button>
    </Action>
</EmptyState>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Icon | IconName? | null | Optional icon |
| Title | string | — | Empty state title |
| Description | string? | null | Description text |
| Action | RenderFragment? | null | CTA button/content |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: EmptyState
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement EmptyState.razor + EmptyState.razor.css

**Commit:** `feat(empty-state): add EmptyState component`

**Dependencies:** Feature 002 (Icon) — optional, can work without it

---

## Testing Strategy

### bUnit Tests
- [ ] Renders title and description
- [ ] Icon renders when provided
- [ ] Action RenderFragment renders
- [ ] Custom CSS class applied

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/EmptyState.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
