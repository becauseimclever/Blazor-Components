# Feature 003: Modal Component
> **Status:** Planning

## Overview

Extract the Modal dialog component from BudgetExperiment. The Modal provides an accessible overlay dialog with configurable title, body content, footer actions, backdrop click handling, keyboard escape support, and focus trapping.

## Problem Statement

### Current State

Modal dialogs exist in BudgetExperiment with proper accessibility (focus trap, ESC close, backdrop). This pattern is needed across all BecauseImClever Blazor apps.

### Target State

A standalone, accessible Modal component in the NuGet package.

---

## User Stories

### US-003-001: Show/Hide Modal
**As a** consuming developer  
**I want to** control modal visibility via a bool parameter  
**So that** I can show/hide the modal programmatically.

**Acceptance Criteria:**
- [ ] Modal renders when IsVisible=true
- [ ] Modal is hidden when IsVisible=false
- [ ] OnClose callback fires when closed

### US-003-002: Modal Content Areas
**As a** consuming developer  
**I want to** provide custom title, body, and footer content  
**So that** I can build different dialog layouts.

**Acceptance Criteria:**
- [ ] Title parameter or TitleContent RenderFragment
- [ ] ChildContent for body
- [ ] FooterContent RenderFragment

### US-003-003: Accessibility
**As a** consuming developer  
**I want** the modal to be accessible  
**So that** keyboard and screen reader users can interact with it.

**Acceptance Criteria:**
- [ ] Focus trapped within modal
- [ ] ESC key closes modal
- [ ] Backdrop click closes modal (configurable)
- [ ] Proper ARIA attributes (role="dialog", aria-modal)

---

## Technical Design

### Component API

```razor
<Modal IsVisible="@_showModal" Title="Confirm Action" OnClose="HandleClose">
    <p>Are you sure?</p>
    <FooterContent>
        <Button Variant="ButtonVariant.Secondary" OnClick="HandleClose">Cancel</Button>
        <Button Variant="ButtonVariant.Primary" OnClick="HandleConfirm">Confirm</Button>
    </FooterContent>
</Modal>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| IsVisible | bool | false | Controls visibility |
| Title | string? | null | Modal title text |
| TitleContent | RenderFragment? | null | Custom title content |
| ChildContent | RenderFragment | — | Modal body |
| FooterContent | RenderFragment? | null | Footer actions |
| CloseOnBackdropClick | bool | true | Close when clicking backdrop |
| CloseOnEscape | bool | true | Close on ESC key |
| OnClose | EventCallback | — | Close callback |
| Size | ModalSize | Medium | Modal width |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: Core Modal
**Tasks:**
- [ ] Write bUnit tests for show/hide, content areas
- [ ] Implement Modal.razor + Modal.razor.css

**Commit:** `feat(modal): add Modal dialog component`

### Phase 2: Accessibility & Keyboard
**Tasks:**
- [ ] Write tests for ESC close, backdrop click, ARIA attributes
- [ ] Implement focus trap and keyboard handling

**Commit:** `feat(modal): add accessibility and keyboard support`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders when IsVisible=true, hidden when false
- [ ] Title renders in header
- [ ] ChildContent renders in body
- [ ] FooterContent renders in footer
- [ ] OnClose fires on backdrop click
- [ ] OnClose fires on ESC key
- [ ] Backdrop click respects CloseOnBackdropClick=false
- [ ] Correct ARIA attributes present

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Modal.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
