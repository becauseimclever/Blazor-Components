# Feature 016: ConsentBanner Component
> **Status:** Planning

## Overview

Extract the ConsentBanner component from the becauseimclever main repo. A GDPR-style cookie/consent banner with animation support, accept/decline actions, and persistent dismissal.

## Problem Statement

### Current State

The ConsentBanner exists in the becauseimclever repo with animation and consent tracking.

### Target State

A ConsentBanner component in the NuGet package for any app needing cookie/privacy consent.

---

## User Stories

### US-016-001: Consent Display
**As a** consuming developer  
**I want to** show a consent banner with accept/decline options  
**So that** my app meets GDPR/privacy requirements.

**Acceptance Criteria:**
- [ ] Banner with customizable message
- [ ] Accept and decline buttons
- [ ] Callbacks for consent decision
- [ ] Slide-in animation
- [ ] Remembers dismissal via callback

---

## Technical Design

### Component API

```razor
<ConsentBanner IsVisible="@_showBanner"
               Message="We use cookies to improve your experience."
               OnAccept="HandleAccept"
               OnDecline="HandleDecline" />
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| IsVisible | bool | false | Controls visibility |
| Message | string | — | Consent message |
| AcceptText | string | "Accept" | Accept button text |
| DeclineText | string | "Decline" | Decline button text |
| OnAccept | EventCallback | — | Accept callback |
| OnDecline | EventCallback | — | Decline callback |
| ChildContent | RenderFragment? | null | Custom content |
| CssClass | string? | null | Additional CSS classes |

---

## Implementation Plan

### Phase 1: ConsentBanner
**Tasks:**
- [ ] Write bUnit tests
- [ ] Implement ConsentBanner.razor + ConsentBanner.razor.css

**Commit:** `feat(consent): add ConsentBanner component`

---

## Testing Strategy

### bUnit Tests
- [ ] Renders when IsVisible=true
- [ ] Hidden when IsVisible=false
- [ ] Message text renders
- [ ] OnAccept fires on accept click
- [ ] OnDecline fires on decline click
- [ ] Custom button text renders

---

## References

- Source: becauseimclever `src/.../ConsentBanner.razor`

---

## Changelog

| Date | Change | Author |
|------|--------|--------|
| 2026-03-09 | Initial draft | @copilot |
