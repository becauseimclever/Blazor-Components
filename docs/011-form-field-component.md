# Feature 011: FormField Component# Feature 011: FormField Component





























































































| 2026-03-09 | Initial draft | @copilot ||------|--------|--------|| Date | Change | Author |## Changelog---- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/FormField.razor`## References---- [ ] CSS error class applied when validation message present- [ ] Help text shown when provided- [ ] Validation message shown when provided- [ ] Required indicator shown when Required=true- [ ] ChildContent renders- [ ] Renders label text### bUnit Tests## Testing Strategy---**Commit:** `feat(form-field): add FormField component`- [ ] Implement FormField.razor + FormField.razor.css- [ ] Write bUnit tests**Tasks:**### Phase 1: FormField## Implementation Plan---| CssClass | string? | null | Additional CSS classes || ChildContent | RenderFragment | — | Input element || ValidationMessage | string? | null | Error message || HelpText | string? | null | Help text below input || Required | bool | false | Shows required indicator || For | string? | null | HTML for attribute || Label | string | — | Field label ||-----------|------|---------|-------------|| Parameter | Type | Default | Description |### Parameters```</FormField>    <input type="email" @bind="_email" /><FormField Label="Email" Required="true" ValidationMessage="@_emailError">```razor### Component API## Technical Design---- [ ] Supports help text- [ ] Supports required indicator- [ ] Displays validation message when provided- [ ] ChildContent slot for the input element- [ ] Renders label with for attribute**Acceptance Criteria:****So that** all forms have consistent field layout.**I want to** wrap form inputs with a label and validation display  **As a** consuming developer  ### US-011-001: Form Field Wrapper## User Stories---A FormField wrapper component in the NuGet package that standardizes label + input + validation display.### Target StateForm fields with labels and validation messages are used throughout BudgetExperiment forms. Each form re-implements this pattern.### Current State## Problem StatementExtract the FormField component from BudgetExperiment. Provides a wrapper for form inputs with label, validation message display, and consistent layout.## Overview> **Status:** Planning> **Status:** Planning

## Overview

Extract the FormField component from BudgetExperiment. A wrapper component that provides consistent form field layout with label, input slot, validation message display, and help text.

## Problem Statement

### Current State

Form fields across BudgetExperiment follow a consistent label + input + validation pattern. This layout wrapper should be reusable.

### Target State

A FormField component in the NuGet package.

---

## User Stories

### US-011-001: Form Field Layout
**As a** consuming developer  
**I want to** wrap form inputs in a consistent layout with label and validation  
**So that** all forms have uniform styling and error display.

**Acceptance Criteria:**
- [ ] Renders label
- [ ] Renders ChildContent (input)
- [ ] Shows validation message when present
- [ ] Optional help text
- [ ] Required indicator on label

---

## Technical Design

### Component API

```razor
<FormField Label="Email" Required="true" ValidationMessage="@_emailError">
    <input @bind="_email" type="email" />
</FormField>
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Label | string | — | Field label |
| Required | bool | false | Show required indicator |
| ValidationMessage | string? | null | Error message |
| HelpText | string? | null | Help text below input |
| ChildContent | RenderFragment | — | Input element |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: FormField
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement FormField.razor + FormField.razor.css

**Commit:** `feat(form-field): add FormField component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders label text
- [ ] Required indicator shows when Required=true
- [ ] ChildContent renders
- [ ] Validation message shows when set
- [ ] Help text renders when provided
- [ ] Error CSS class applied when validation message present

---

## References

- Source: BudgetExperiment `src/BudgetExperiment.Client/Components/Common/FormField.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
