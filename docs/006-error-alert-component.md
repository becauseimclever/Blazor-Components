# Feature 006: ErrorAlert Component
> **Status:** Planning

## Overview

Extract the ErrorAlert component from BudgetExperiment. Displays error messages with optional retry and dismiss actions, conditional rendering based on error state.

## Problem Statement

### Current State

Error display is a universal concern. BudgetExperiment has an ErrorAlert with retry/dismiss callbacks.

### Target State

A reusable ErrorAlert in the NuGet package.

---

## User Stories

### US-006-001: Display Error
**As a** consuming developer  
**I want to** show an error message with optional retry/dismiss buttons  
**So that** users can see what went wrong and take action.

**Acceptance Criteria:**
- [ ] Renders error message
- [ ] Optional retry button with OnRetry callback
- [ ] Optional dismiss button with OnDismiss callback
- [ ] Conditional rendering (only shows when error message is set)

---

## Technical Design

### Component API

```razor
<ErrorAlert Message="@_errorMessage"
            OnRetry="LoadData"
            OnDismiss="ClearError" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Message | string? | null | Error message (null = hidden) |
| OnRetry | EventCallback? | null | Retry callback |
| OnDismiss | EventCallback? | null | Dismiss callback |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: ErrorAlert
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement ErrorAlert.razor + ErrorAlert.razor.css

**Commit:** `feat(error-alert): add ErrorAlert component`

---

## Testing Strategy

### bUnit Tests
- [ ] Hidden when Message is null/empty
- [ ] Renders message text
- [ ] Retry button visible when OnRetry provided
- [ ] Dismiss button visible when OnDismiss provided
- [ ] OnRetry callback fires
- [ ] OnDismiss callback fires

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/ErrorAlert.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
