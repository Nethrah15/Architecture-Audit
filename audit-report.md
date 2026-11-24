# SWYNEX Accessibility Audit Report - Task 2
**Site Audited:** https://www.irctc.co.in (Public Service Website)
**Date:** 24 Nov 2026 | **Browser:** Chrome Desktop
**Method:** 1) Lighthouse Audit 2) Keyboard-Only Pass - No Mouse Used

## Lighthouse Scores - Baseline Evidence
| Category | Score |
| :--- | :--- |
| Performance | 42 / 100 |
| Accessibility | 78 / 100 |
| Best Practices | 71 / 100 |
| SEO | 84 / 100 |

Screenshot: See `docs/screenshots/lighthouse.png` - Lighthouse report generated in Chrome DevTools > Lighthouse > Analyze page load.

## Keyboard Navigation Test
Test: Tab from Home -> Login -> From Station -> Date Picker -> Search
Result: FAILED - User gets stuck in Date Picker, focus disappears, Login modal cannot be closed with Esc.

## 5 Issues With Evidence + Fix

### Issue 1 - Low Colour Contrast - P0 HIGH
Evidence: Grey text #8A8A8A on white background in train list and footer. Lighthouse error: "Background and foreground colors do not have sufficient contrast ratio"
WCAG: 1.4.3 Contrast Minimum (AA) - Needs 4.5:1, site has 2.8:1
Impact: Low-vision users cannot read fare/timing.
Fix in SWYNEX: Use #1A1A1A text on #FFFFFF. Check all text with contrast checker.

### Issue 2 - Images Without Alt Text - P0 HIGH
Evidence: 14 banner images have alt="" or missing alt. Lighthouse error: "Image elements do not have [alt] attributes"
WCAG: 1.1.1 Non-text Content (A)
Impact: Screen reader says "image" with no meaning.
Fix in SWYNEX: Every <img> must have alt="description". Add eslint-plugin-jsx-a11y.

### Issue 3 - No Skip To Main Content Link - P1 MEDIUM
Evidence: Pressing Tab goes through 40+ header links before reaching main search. No skip link in code.
WCAG: 2.4.1 Bypass Blocks (A)
Impact: Keyboard users waste 2 minutes every page.
Fix in SWYNEX: Added in App.jsx: <a href="#main">Skip to main content</a>

### Issue 4 - Form Inputs Without Labels - P0 HIGH
Evidence: From, To, Date inputs use placeholder only, no <label>. Screen reader says "edit text blank".
WCAG: 1.3.1 Info and Relationships + 3.3.2 Labels (A)
Impact: Blind users cannot fill form.
Fix in SWYNEX: Every input has <label htmlFor="...">. Never use placeholder as label.

### Issue 5 - Keyboard Trap + No Visible Focus - P0 CRITICAL
Evidence: Date picker traps Tab key, cannot exit. CSS has outline:none, so focus invisible.
WCAG: 2.1.2 No Keyboard Trap + 2.4.7 Focus Visible (A/AA)
Impact: Full blocker - keyboard-only user is stuck.
Fix in SWYNEX: Never use outline:none. Use :focus-visible { outline: 2px solid #2563EB; }. Modal closes with Esc.

## Priority Summary
P0 Critical: Issue 5, 1, 2, 4
P1 Medium: Issue 3

## How SWYNEX Fixes It
Semantic HTML, visible focus, labels, alt text, 4.5:1 contrast, full keyboard test before commit.
