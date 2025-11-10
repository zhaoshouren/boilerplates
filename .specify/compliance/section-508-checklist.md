# Section 508 Compliance Checklist

This checklist is intended to support teams delivering public-facing web properties that must meet Section 508 accessibility requirements. Use this as a release artifact: attach automated scan results, manual audit notes, remediation tickets, and a signer (accessibility owner) before a 508-scoped release.

---

## Meta
- Product / App:
- Release / Build:
- In-scope (yes/no):
- Accessibility Owner / Reviewer:
- Date of Audit:

---

## Acceptance Criteria (high-level)
- All critical user journeys for the release are keyboard-navigable and accessible to screen readers.
- All multimedia content has captions or transcripts.
- Forms are properly labeled and usable via keyboard and screen reader.
- No critical or high-severity automated scanner findings remain open without a documented mitigation and timeline.
- A completed manual audit checklist and signed acceptance by the accessibility owner are present.

---

## Automated Checks (run in CI where possible)
- Axe / pa11y / Lighthouse accessibility audit: run and attach report (include JSON and HTML output).
- Color contrast checks for all UI themes (WCAG AA contrast ratio >= 4.5:1 for normal text, 3:1 for large text).
- Presence of language attribute on HTML `<html lang="...">`.
- Ensure interactive elements have discernible names (accessible name computation).
- Ensure images have `alt` text or proper role/hidden if decorative.
- Verify ARIA attributes are valid and used only when necessary.

---

## Manual / Exploratory Checks (guidance for auditors)
- Keyboard-only navigation:
  - Can the user access all interactive controls using Tab/Shift+Tab, and activate them (Enter/Space) without a mouse?
  - Is the focus order logical and predictable?
  - Are focus styles visible and not removed?
- Screen reader walkthrough (VoiceOver on macOS, NVDA on Windows):
  - Confirm reading order matches visual order.
  - Verify meaningful alternative text for images, ARIA labels/roles for complex widgets.
  - Validate that form fields announce labels, hints, and error messages.
- Multimedia:
  - Video captions present and synchronized; audio has transcript.
  - Live captioning or real-time alternatives documented for live events where required.
- Forms & Validation:
  - Each input has an associated label or aria-label.
  - Error messages are programmatically associated with inputs (aria-describedby) and clear guidance is provided to fix errors.
- Focus management and modals:
  - Focus is trapped inside modals while open and returned to the invoking control on close.
  - Dialogs and menus are announced to assistive tech.
- Skip links and landmarks:
  - Provide "skip to main content" link and use landmark elements (header, nav, main, footer) where appropriate.
- Tables and complex components:
  - Data tables have headers and summary where needed; complex grids expose proper roles and keyboard interactions.
- PDF and non-HTML content:
  - PDFs included in the release are tagged and accessible or accompanied by an accessible alternative.
- CAPTCHAs and human verification:
  - Avoid inaccessible CAPTCHAs; provide an accessible alternative (e.g., challenge via phone, email, or accessible logic) and document it.

---

## Testing Steps (recommended minimal flow)
1. Run automated scanners (axe/pa11y/Lighthouse) and save outputs to the release artifact folder.
2. Run color contrast tool against built CSS variables / themes.
3. Perform keyboard-only smoke test for critical journeys (login, search, checkout, profile update, error flows).
4. Perform screen reader smoke test on at least one platform (macOS VoiceOver or Windows NVDA) for the same critical journeys.
5. Manual checklist: auditor fills the manual audit items above and attaches notes/screenshots for failures.
6. Open remediation items in the issue tracker and link them to the release artifact; mark severity and assignee.
7. Accessibility owner signs off the checklist (name, date) or rejects with remediation plan.

---

## Severity Guidance (example)
- Critical: prevents core workflow for assistive tech users (e.g., login impossible via keyboard, missing captions for required multimedia).
- High: major usability problem but a partial workaround exists (e.g., form fields not announced properly, major color contrast failures).
- Medium: affects non-critical journeys or causes confusion but not complete block (e.g., missing alt text on non-critical images).
- Low: minor improvements or suggestions (e.g., microcopy clarity, minor aria refinements).

---

## Evidence & Artifacts (what to attach)
- Automated scan reports (axe/pa11y/Lighthouse JSON/HTML).
- Screen reader test notes and audio/video recordings if helpful.
- Keyboard smoke test checklist with step pass/fail and screenshots.
- Issue tracker links for remediation items with acceptance criteria.
- Final signed checklist by accessibility owner.

---

## CI Integration (optional)
- Add an accessibility job that runs on PRs and main branch builds that does:
  - `npm run lint:accessibility` (or equivalent)
  - `axe` or `pa11y` scan against built pages
  - upload artifacts to the build/release
- Configure thresholding so that new critical/high violations block merges; lower-severity failures create warnings/tickets.

---

## Exception & Mitigation Log
- If an issue cannot be resolved before release, document:
  - Issue id / description
  - Impact and affected journeys
  - Mitigation and compensating controls
  - Owner and target remediation date
  - Approval by accessibility owner

---

## Sign-off
- Accessibility owner: ____________________  Date: ___________
- QA lead: ________________________________  Date: ___________
- Product owner: __________________________ Date: ___________

---

## Notes & References
- Section 508 Refresh: https://www.access-board.gov/ict/
- WCAG 2.1: https://www.w3.org/TR/WCAG21/
- Axe Core: https://github.com/dequelabs/axe-core
- Pa11y: https://pa11y.org/
- Lighthouse accessibility audits: https://developers.google.com/web/tools/lighthouse

