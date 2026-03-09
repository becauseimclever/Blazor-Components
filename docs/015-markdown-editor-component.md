# Feature 015: MarkdownEditor Component
> **Status:** Planning

## Overview

Extract the MarkdownEditor component from the becauseimclever main repo. A rich markdown editing component with toolbar, live preview, and undo/redo support.

## Problem Statement

### Current State

The MarkdownEditor exists in the becauseimclever repo. It provides a rich editing experience for markdown content with toolbar buttons, preview mode, and edit history.

### Target State

A MarkdownEditor component in the NuGet package.

---

## User Stories

### US-015-001: Markdown Editing
**As a** consuming developer  
**I want to** embed a markdown editor with toolbar and preview  
**So that** users can write and preview markdown content.

**Acceptance Criteria:**
- [ ] Text area for markdown input
- [ ] Toolbar with common formatting buttons (bold, italic, heading, link, list)
- [ ] Live preview panel
- [ ] Two-way binding via Value/ValueChanged
- [ ] Undo/redo support

### US-015-002: Editor Modes
**As a** consuming developer  
**I want to** switch between edit, preview, and split modes  
**So that** users can choose their preferred editing experience.

**Acceptance Criteria:**
- [ ] Edit-only mode
- [ ] Preview-only mode
- [ ] Side-by-side split mode

---

## Technical Design

### Component API

```razor
<MarkdownEditor @bind-Value="_content" Mode="EditorMode.Split" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| Value | string | — | Markdown content |
| ValueChanged | EventCallback<string> | — | Content change callback |
| Mode | EditorMode | Split | Editor display mode |
| Placeholder | string? | null | Placeholder text |
| MinHeight | string | "200px" | Minimum editor height |
| ReadOnly | bool | false | Read-only mode |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: Core Editor
**Tasks:**
- [ ] Write bUnit tests for rendering, two-way binding
- [ ] Implement MarkdownEditor.razor + MarkdownEditor.razor.css

**Commit:** `feat(markdown): add MarkdownEditor component`

### Phase 2: Toolbar & Preview
**Tasks:**
- [ ] Write tests for toolbar actions, preview rendering
- [ ] Implement toolbar and markdown-to-HTML preview

**Commit:** `feat(markdown): add toolbar and preview support`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders textarea with value
- [ ] ValueChanged fires on input
- [ ] Toolbar buttons insert correct markdown
- [ ] Editor modes show/hide panels correctly
- [ ] ReadOnly disables editing

---

## References

- Source: becauseimclever `src/.../MarkdownEditor.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
