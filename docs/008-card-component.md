# Feature 008: Card Component
> **Status:** Planning

## Overview

Extract the Card component from BudgetExperiment. A container component providing a styled card with optional header, body, and footer sections.

## Problem Statement

### Current State

Cards are used extensively in BudgetExperiment for grouping content. The Card component should be reusable.

### Target State

A Card component in the NuGet package.

---

## User Stories

### US-008-001: Card Layout
**As a** consuming developer  
**I want to** wrap content in a styled card container  
**So that** I can group related content visually.

**Acceptance Criteria:**
- [ ] Renders ChildContent in card body
- [ ] Optional HeaderContent and FooterContent
- [ ] Optional Title shorthand parameter
- [ ] Supports CSS class customization

---

## Technical Design

### Component API

```razor
<Card Title="Summary">
    <p>Card content here</p>
</Card>

<Card>
    <HeaderContent>Custom header</HeaderContent>
    <ChildContent>Body</ChildContent>
    <FooterContent>Footer actions</FooterContent>
</Card>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Title | string? | null | Card title |
| HeaderContent | RenderFragment? | null | Custom header |
| ChildContent | RenderFragment | — | Card body |
| FooterContent | RenderFragment? | null | Card footer |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: Card
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement Card.razor + Card.razor.css

**Commit:** `feat(card): add Card component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders ChildContent
- [ ] Title renders in header
- [ ] HeaderContent overrides Title
- [ ] FooterContent renders
- [ ] Custom CSS class applied

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/Card.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
