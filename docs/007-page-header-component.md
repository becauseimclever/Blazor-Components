# Feature 007: PageHeader Component
> **Status:** Planning

## Overview

Extract the PageHeader component from BudgetExperiment. Provides a consistent page header with title, optional subtitle, back button, and action slot for page-level buttons.

## Problem Statement

### Current State

Every page in BudgetExperiment uses a PageHeader for consistent layout. This pattern should be reusable.

### Target State

A PageHeader component in the NuGet package.

---

## User Stories

### US-007-001: Page Header Layout
**As a** consuming developer  
**I want to** add a consistent page header with title, subtitle, and actions  
**So that** all pages have uniform top-level navigation.

**Acceptance Criteria:**
- [ ] Renders title
- [ ] Optional subtitle
- [ ] Optional back button with navigation callback
- [ ] Actions slot for page-level buttons

---

## Technical Design

### Component API

```razor
<PageHeader Title="Accounts" Subtitle="Manage your accounts">
    <Actions>
        <Button OnClick="AddAccount">Add Account</Button>
    </Actions>
</PageHeader>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Title | string | — | Page title |
| Subtitle | string? | null | Optional subtitle |
| ShowBackButton | bool | false | Show back navigation |
| OnBack | EventCallback | — | Back button callback |
| Actions | RenderFragment? | null | Action buttons slot |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: PageHeader
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement PageHeader.razor + PageHeader.razor.css

**Commit:** `feat(page-header): add PageHeader component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders title text
- [ ] Renders subtitle when provided
- [ ] Back button visible when ShowBackButton=true
- [ ] OnBack fires on back button click
- [ ] Actions RenderFragment renders

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/PageHeader.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
