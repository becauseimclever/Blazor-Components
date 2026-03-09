# Feature 004: ConfirmDialog Component
> **Status:** Planning

## Overview

Extract the ConfirmDialog component from BudgetExperiment. This is a specialized Modal that provides a standard confirm/cancel dialog pattern with customizable message, confirm button variant (danger for destructive actions), and callbacks.

## Problem Statement

### Current State

Confirmation dialogs are a common pattern across BecauseImClever projects. BudgetExperiment has a ConfirmDialog component built on top of Modal.

### Target State

A ConfirmDialog component in the NuGet package that composes the Modal component for quick confirm/cancel interactions.

---

## User Stories

### US-004-001: Confirm/Cancel Flow
**As a** consuming developer  
**I want to** show a confirmation dialog with a message and two buttons  
**So that** users explicitly confirm destructive or important actions.

**Acceptance Criteria:**
- [ ] Renders message, Confirm button, Cancel button
- [ ] OnConfirm callback fires on confirm click
- [ ] OnCancel callback fires on cancel click
- [ ] Confirm button variant is configurable (danger for deletes)

---

## Technical Design

### Component API

```razor
<ConfirmDialog IsVisible="@_showConfirm"
               Title="Delete Item"
               Message="Are you sure you want to delete this item? This cannot be undone."
               ConfirmText="Delete"
               ConfirmVariant="ButtonVariant.Danger"
               OnConfirm="HandleDelete"
               OnCancel="() => _showConfirm = false" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| IsVisible | bool | false | Controls visibility |
| Title | string | "Confirm" | Dialog title |
| Message | string | — | Confirmation message |
| ConfirmText | string | "Confirm" | Confirm button text |
| CancelText | string | "Cancel" | Cancel button text |
| ConfirmVariant | ButtonVariant | Primary | Confirm button style |
| OnConfirm | EventCallback | — | Confirm callback |
| OnCancel | EventCallback | — | Cancel callback |

---

## Implementation Plan

### Phase 1: ConfirmDialog
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement ConfirmDialog.razor (composing Modal + Button)

**Commit:** `feat(confirm-dialog): add ConfirmDialog component`

**Dependencies:** Feature 001 (Button), Feature 003 (Modal)

---

## Testing Strategy

### bUnit Tests
- [ ] Renders title and message
- [ ] Confirm button fires OnConfirm
- [ ] Cancel button fires OnCancel
- [ ] Confirm button uses specified variant
- [ ] Custom button text renders

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/ConfirmDialog.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
