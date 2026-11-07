
Accessibility Audit — HackTrent @ Trent University
Target: WCAG 2.2 Level AA (low-risk AAA where free). Scope: the whole repository: index.html, styles.css, reset.css, assets/. Stack: static HTML + plain CSS. No build step, no JS framework, no package.json, so there are no build, lint or test commands to baseline.

1. Executive summary
Severity	Found	Fixed	Needs human decision
Blocker	3	3	0
Critical	5	5	0
Major	9	9	0
Minor	8	8	0
AAA-Enhancement	2	2	0
Product / content decisions	4	0 (safe fallback in place)	4
Total	31	27	4
Most common WCAG criteria: 1.4.3 Contrast (7 colour pairs failed, some as low as 1.10:1), 1.3.1 Info & Relationships, 2.1.1 Keyboard, 4.1.2 Name/Role/Value, 1.1.1 Non-text Content, 1.4.10 Reflow.

Design approach: pass 1 kept the original brand backgrounds and fixed only the foreground colours (AA). Pass 2 (section 6) then deliberately went further on colour and typography, to AAA contrast, calmer colours, light/dark themes and a low-vision typeface, while keeping the violet / purple / aqua identity.

2. Issue log
Legend: Status = Fixed / Needs Human Decision.

ID	Severity	File / component	Users affected	WCAG 2.2	Before (root cause)	Fix implemented	Status
A11Y-001	Blocker	index.html nav	Keyboard, switch, screen-reader, voice-control users	2.1.1 (A), 4.1.2 (A), 1.3.1 (A)	Navigation items were <div onclick="location.href=…">. They could not be focused, had no link role, could not be activated by keyboard, and were invisible to SR link lists and voice commands.	Replaced with <nav aria-label="Main"><ul><li><a href> using native links. Same hrefs. aria-current="page" on Home.	Fixed
A11Y-002	Blocker	index.html structure	Everyone; SR and keyboard most	1.3.1 (A), 1.3.2 (A)	The <div> and <section class="content"> were never closed, so the right sidebar and the footer were parsed inside the main content. The grid placed them in the centre column, leaving column 3 empty (visible in the baseline). DOM and visual order were broken.	Closed elements correctly. Each region is now a direct grid child in the order header → nav → main → aside → footer, which matches visual and tab order. The page now renders the three-column layout the stylesheet always intended.	Fixed
A11Y-003	Blocker	styles.css all regions	Low vision, CVD, older users, anyone in sunlight	1.4.3 (AA)	7 text/background pairs failed, e.g. nav blue on purple 1.10:1, body dark blue on black 1.37:1, footer purple on blueviolet 1.58:1.	Kept all backgrounds. New foregrounds: white on blueviolet 5.96:1, white on purple 9.42:1, light blue on black 13.74:1, indigo on aqua 10.33:1. Centralised as :root tokens.	Fixed
A11Y-004	Critical	index.html images	Blind / SR users; users with images off	1.1.1 (A)	Both <img> had no alt, so SRs read the filename ("happyHackers dot JPG").	Descriptive alt on both photos, written from the actual image content.	Fixed
A11Y-005	Critical	Search bar	SR, voice-control, cognitive, mobile	1.3.1 (A), 3.3.2 (A), 4.1.2 (A), 1.3.5 (AA)	The input had no programmatic label (<b>Search:</b> was plain text) and relied on a placeholder. type="url" gave the wrong mobile keyboard. There was no <form> or search landmark, and the button had no type.	<form role="search"> with <label for> ("Search:", same look), type="search", name="q", enterkeyhint="search", <button type="submit">, and a hidden label extension so the button reads "Go, search the site".	Fixed
A11Y-006	Critical	Footer links	Low vision, CVD	1.4.3 (AA), 1.4.1 (A)	Default link blue on blueviolet measured 1.58:1 (1.12:1 under protanopia).	White links (5.96:1) that keep their underline, so links are distinguishable without colour.	Fixed
A11Y-007	Critical	Whole page	Everyone (rendering)	4.1.1/robustness (best practice)	No <!DOCTYPE html>, so the page rendered in quirks mode with browser-dependent layout.	Added the doctype (standards mode).	Fixed
A11Y-008	Critical	Search field boundary and placeholder	Low vision	1.4.11 (AA), 1.4.3 (AA)	Default grey input border on blueviolet was 1.31:1, and the placeholder #757575 on aqua was 3.67:1.	The aqua field fill against the header (4.75:1) identifies the control's boundary, so the border was left as is. Placeholder darkened to #3B3B3B (8.93:1). The visible "Search:" label means the placeholder is no longer the only cue.	Fixed
A11Y-009	Major	Landmarks	SR users navigating by region	1.3.1 (A), 2.4.1 (A)	Everything was a generic <section>, <div> or <span>. There were no banner, navigation, main, complementary or contentinfo landmarks.	<header>, <nav aria-label="Main">, <main id="main-content">, <aside aria-labelledby>, <footer>, <nav aria-label="Legal">.	Fixed
A11Y-010	Major	Skip link	Keyboard, switch, SR	2.4.1 (A)	Keyboard users had to tab through the search and all five nav links on every page.	Added "Skip to main content" as the first focusable element. It is hidden until focused, then slides in at high contrast (12.95:1) and moves focus to <main tabindex="-1">. The slide is disabled under prefers-reduced-motion.	Fixed
A11Y-011	Major	Headings	SR users (heading navigation), cognitive	1.3.1 (A), 2.4.6 (AA)	There was no <h1>. Levels were picked for size (h2 → h3 → h4), the tagline "@ Trent University" was a heading, and the global h2/h3 rules styled every heading on the page.	<h1> HackTrent, with the tagline as <p>. "Main content" is now <h2>, the sidebar has <h2> "Latest news" (visually hidden) and the news item is <h3>. Styles moved to classes (.site-title, .site-tagline) so rank and look are decoupled with identical rendering.	Fixed
A11Y-012	Major	Focus visibility	Keyboard, low vision	2.4.7 (AA), 2.4.11 (AA), 2.4.13 (AAA)	The browser default ring was nearly invisible on purple and blueviolet.	Global :focus-visible dual-tone ring: 3px white outline plus a black halo from box-shadow, so ≥3:1 on every background in the palette (purple 9.42, blueviolet 5.96/3.52, aqua 16.75, black 21). Mouse clicks show no ring. In Windows High Contrast the outline remains (forced-colors safe).	Fixed
A11Y-013	Major	Layout at zoom / mobile	Low vision (zoom), mobile, reflow	1.4.10 (AA), 1.4.4 (AA)	Fixed 220px 1fr 220px grid plus a width="500px" image forced horizontal scrolling at 320 CSS px (400% zoom).	Added a single-column breakpoint at max-width: 48em (em-based, so it also triggers on text zoom). Images use max-width:100%; height:auto. The header wraps.	Fixed
A11Y-014	Major	Page title	SR, multi-tab users, cognitive	2.4.2 (A)	<title> was "Accessbility Challenge" (misspelt, not descriptive of the page).	"Home – HackTrent @ Trent University".	Fixed
A11Y-015	Major	Nav text colour	Low vision, CVD	1.4.3 (AA)	Blue on purple measured 1.10:1.	White on purple, 9.42:1 (10.53:1 under protanopia).	Fixed
A11Y-016	Major	Main content text	Low vision, CVD	1.4.3 (AA)	Dark blue on black measured 1.37:1.	Light blue #ADD8E6 on black, 13.74:1 (AAA), keeping the blue-on-black design intent.	Fixed
A11Y-017	Major	Header title and labels	Low vision, CVD	1.4.3 (AA)	Purple on blueviolet measured 1.58:1, and the black "Search:" label 3.52:1.	White, 5.96:1.	Fixed
A11Y-018	Minor	Target size	Motor impairment, touch, tremor	2.5.8 (AA)	Nav, footer and news links were only as tall as their text (≈19px), and the "Go" button was about 21px.	Nav links are display:block with padding-block:.5em cancelled by margin-block:-.5em, so the hit area grows to 188×35 with zero visual shift. Footer and news links use the same trick (19px → 27px). The search input and Go button have min-block-size: 1.5rem (24px).	Fixed
A11Y-019	Minor	Footer separators	SR users	1.3.1 (A)	The visible "|" characters were announced as "vertical bar".	Links are now a <ul> in <nav aria-label="Legal">. The pipes are kept visually inside aria-hidden="true" spans.	Fixed
A11Y-020	Minor	News date	SR, assistive parsing	1.3.1 (A)	The date was plain text in a <p>.	<time datetime="2025-11-15">.	Fixed
A11Y-021	Minor	<img width="500px">	Everyone (layout shift), low-bandwidth	best practice / 2.2.2-adjacent	Invalid width values (with units) and no height caused layout shift while the large images loaded.	Valid width and height attributes from the real aspect ratio, plus decoding="async".	Fixed
A11Y-022	Minor	Stylesheet paths	Everyone when hosted under a sub-path or opened from disk	4.1 robustness	/reset.css and /styles.css (root-absolute) broke all styling on GitHub Pages project sites and file://, leaving an unstyled page.	Relative paths.	Fixed
A11Y-023	Minor	<br> used for spacing	SR (announces blank), text-spacing users	1.3.1 (A)	A <br> between paragraphs was used purely for spacing.	Removed and replaced with equivalent CSS margin.	Fixed
A11Y-024	Minor	Lorem ipsum language	SR users (pronunciation)	3.1.2 (AA)	Latin placeholder text was read with English pronunciation rules.	lang="la" on the placeholder paragraphs until real copy arrives (see HD-002).	Fixed
A11Y-025	Minor	Presentational <b> / <i>	SR users	1.3.1 (A)	<b> and <i> were used for styling, and reset.css already neutralised their visual effect.	Removed with no visual change (the reset already rendered them as normal text).	Fixed
A11Y-026	AAA-Enhancement	Huge images	Low-bandwidth, cognitive (slow loads), SR (long busy states)	—	A 4.1 MB, 4898px photo was shown at 500px, and a 2.6 MB, 4032px photo at 100px.	Generated -1000w and -200w copies (2× density) in assets/. The originals are kept.	Fixed
A11Y-027	AAA-Enhancement	Motion	Vestibular disorders	2.3.3 (AAA)	The site has no animation. The only new motion is the skip-link slide.	Wrapped in @media (prefers-reduced-motion: no-preference). Smooth anchor scrolling is likewise opt-in.	Fixed
Verified non-issues (checked, nothing to fix)
1.4.1 Use of Colour: there are no colour-only states. Every link (prose, sidebar, footer) keeps an underline. The current nav item gets a thicker underline, not only a colour.
1.4.12 Text Spacing: no fixed heights or overflow:hidden on text containers. The page was tested with the text-spacing bookmarklet values (section 4).
2.1.2 No Keyboard Trap / 2.4.3 Focus Order: there are no widgets or modals, and tab order follows DOM order, which matches visual order.
Orientation (1.3.4): not locked.
Media / carousels / iframes / forms beyond search: none exist in the repo.
3. Human / product decision log
ID	Item	Why it needs a human	Safe fallback in place
HD-001	In-page links point to anchors that don't exist: #home, #about, #apply, #schedule, #contact, #image, #privacy, #terms, #accessibility. Activating them does nothing, which confuses SR and keyboard users (2.4.4 / 3.2.x).	Those pages or sections have not been written yet.	Links stay as real, focusable <a> elements so they will work the moment the targets exist. #main-content is the only anchor guaranteed to exist.
HD-002	Placeholder copy: "Main content", "this is the right content", lorem ipsum. These fail 2.4.6 (descriptive headings) in spirit.	Needs real editorial content.	Headings and landmarks are already correctly structured, and the lorem text is tagged lang="la". A visually hidden "Latest news" heading names the sidebar.
HD-003	Search does nothing. The form submits ?q= back to the same page with no results.	Needs a search backend or a client-side index.	The form is semantically complete (landmark, label, submit). Once results exist, announce the count in a role="status" region.
HD-004	Accessibility statement page (footer link) does not exist.	Needs an owner and contact channel (email/phone) for accessibility feedback.	The footer link is in place and correctly labelled.
4. Verification evidence
All checks were run on 2026-11-07 against the page served locally (.claude/launch.json, a tiny Node static server) in the Claude desktop browser pane (Chromium).

4.1 Build / validity
There is no build system. The page now parses in standards mode (document.compatMode === "CSS1Compat"; before: quirks).

4.2 Automated scan: axe-core 4.10.2
Rules: wcag2a, wcag2aa, wcag21a, wcag21aa, wcag22aa, best-practice.

Violations	Failing nodes
Before	5 rules: color-contrast (serious ×16), image-alt (critical ×2), landmark-one-main, page-has-heading-one, region (×5)	26
After	0 (0 incomplete, 48 passes)	0
4.3 Mathematical contrast audit
The script (contrast.js, WCAG relative luminance plus Machado 2009 CVD matrices at full severity) was run over every foreground/background pair.

Pair	Before	After	Protan / Deutan / Tritan (after)
Header title and tagline / blueviolet	1.58 (purple)	5.96 (white)	5.33 / 5.45 / 5.57
"Search:" label / blueviolet	3.52 (black)	5.96	5.33 / 5.45 / 5.57
Placeholder / aqua	3.67	8.93	10.00 / 8.25 / 8.93
Input fill vs header (1.4.11)	—	4.75	4.76 / 4.02 / 4.44
Nav links / purple	1.10 (blue)	9.42 (white)	10.53 / 8.65 / 9.15
Main text / black	1.37 (darkblue)	13.74 (lightblue)	14.32 / 13.37 / 13.79
Sidebar link / aqua	7.49	10.33 (indigo)	10.54 / 8.83 / 9.88
Footer text and links / blueviolet	1.58 / 1.58	5.96	5.33 / 5.45 / 5.57
Skip link (white / indigo)	—	12.95	11.81 / 11.99 / 12.39
Focus ring vs every background	—	≥ 3.52 (worst: black halo on blueviolet; the white inner ring there is 5.96)	all ≥ 3
Greyscale/achromatopsia: every pair keeps a ≥5.96:1 luminance difference, so no hierarchy is lost.

4.4 Keyboard walk-through
Tab order: Skip link → search input → Go → Home → About Us → Apply Now → Event Schedule → Contact → HackTrent2025 Recap → Privacy Policy → Terms of Use → Accessibility. This matches the visual order. Pressing Enter on the skip link moved document.activeElement to #main-content, and the next Tab landed on the first link after main. Focus rings were confirmed visible in screenshots on blueviolet, purple and aqua. No traps.

4.5 Target sizes (measured bounding boxes)
Skip link 182×43 · search input 146×24 · Go 37×25 · nav links 188×35 · news and footer links 27px tall. All ≥ 24×24.

4.6 Reflow, zoom and text spacing
320 CSS px (≈400% zoom): scrollWidth === innerWidth === 320, so there is no horizontal scroll. The layout stacks header → nav (wrapping row) → main → news → footer.
Text-spacing override (1.4.12) (line-height 1.5, letter .12em, word .16em, paragraph 2em) at 320px: no horizontal overflow, no clipped text. The only element reported as clipped is the intentionally visually-hidden "Latest news" heading.
The breakpoint is in em, so it also triggers when the user enlarges the default font size.
4.7 Motion
The only motion is the skip-link slide and smooth anchor scrolling, both inside @media (prefers-reduced-motion: no-preference). With reduce enabled the skip link appears instantly.

4.8 Not verifiable here (do before release)
Real screen-reader passes (NVDA + Firefox, JAWS + Chrome, VoiceOver iOS/macOS, TalkBack). The structure and names are verified via the accessibility tree and axe, but not by ear.
Windows High Contrast / forced-colors. The focus indicator uses outline, which survives forced colours, but this was not visually tested on Windows.
5. Preventative recommendations
Keep semantics in HTML, styling in classes. Never style bare element selectors like h2 { } or section:first-child { }. That is what pushed the original markup to pick heading levels by size.
Colour tokens only. All colours live in :root in styles.css, each annotated with its measured contrast ratio. Any new pairing should be checked (e.g. with WebAIM's contrast checker) before merging.
Automated checks in CI. Add npx @axe-core/cli or pa11y-ci against the served page in a GitHub Action, and fail on any serious or critical violation.
Validate HTML. Run npx html-validate index.html in CI. It would have caught the unclosed tags (A11Y-002), the missing doctype and the invalid width values.
Manual pass per release. Tab through the whole page, use VoiceOver rotor or NVDA elements list for headings and landmarks, zoom to 400%, and turn on reduce motion and Windows High Contrast.
Images. Add alt at the time an image is added. Export web-sized images (≤2× display size) rather than straight-from-camera files.
6. Pass 2 — AAA colour and typography redesign (2026-11-07)
Requested as a follow-up: "make the website as accessible as possible", with the look allowed to change. The previous AA stylesheet is kept for comparison at /private/tmp/…/scratchpad/styles-aa.css. That is a session temp file, so copy it if you want to keep it.

6.1 Changes
ID	Change	Who it helps	WCAG
A11Y-028	Header and footer violet deepened #8A2BE2 → #5E1A9E. White text goes from 5.96 → 9.92:1.	Low vision, CVD	1.4.6 (AAA)
A11Y-029	Neon aqua sidebar and search field → soft aqua #CFF7F7. Text 14.86:1, links 11.29:1.	Photophobia, migraine, Irlen / visual stress, dyslexia	1.4.6 (AAA), 1.4.8
A11Y-030	Main area follows the system light / dark setting (prefers-color-scheme). Light: #1A1A2E on #FAF8FF (16.19:1). Dark: #EDEAF5 on soft dark #14101F (15.74:1) instead of pure black, which reduces halation for people with astigmatism. Dark sidebar is deep teal #0F3A3D (11.47:1 text, 9.57:1 links). color-scheme meta and property set so form controls and scrollbars match.	Light-sensitive users, astigmatism, low vision	1.4.6 (AAA), 1.4.8 (AAA)
A11Y-031	The header and nav are now nearly the same luminance (1.05:1), so a 4px aqua divider separates them (7.91:1 vs header, 7.51:1 vs nav). The footer gets the same line.	Low vision, cognitive (clear regions)	1.3.1 support
A11Y-032	Typeface: Atkinson Hyperlegible Next, designed by the Braille Institute for low-vision readers, with distinct I / l / 1 and O / 0. It replaces the browser-default Times, with a system sans-serif fallback.	Low vision, dyslexia	1.4.8 (AAA)
A11Y-033	Base size 16 → 18px (1.125rem, scales with user settings). Line height 1.5. Form controls inherit the font, so the search box is no longer 13px.	Low vision, older users	1.4.4, 1.4.8 (AAA)
A11Y-034	Body paragraphs left-aligned (were centred) with a 65ch maximum line length. Headings stay centred.	Dyslexia, cognitive, screen magnifier users	1.4.8 (AAA)
A11Y-035	Visual hierarchy fixed: the title "HackTrent" is now the largest text (2.25em, bold), and "@ Trent University" is secondary (1.25em). Before, the tagline was larger than the title.	Cognitive, low vision	1.3.1 support, 2.4.6
A11Y-036	The current nav item is also bold (plus the thick underline), so there are two non-colour cues.	CVD, low vision	1.4.1
A11Y-037	Search input and button have a visible 1px dark border (14.86:1). Spacing around everything is more generous.	Low vision, motor	1.4.11
6.2 Verification (pass 2)
Check	Light theme	Dark theme
axe-core 4.10.2 incl. wcag2aaa (enhanced 7:1 contrast)	0 violations, 0 incomplete, 50 passes	0 violations, 0 incomplete, 50 passes
320px reflow: scrollWidth	320 (no horizontal scroll)	—
320px + 1.4.12 text-spacing override	320 (no overflow)	—
Target heights (px)	skip 54 · input/Go 38 · nav 45 · inline links 36	—
Font actually loaded	document.fonts.check = true	—
Every text pair is ≥ 7:1 and was also checked under simulated protanopia, deuteranopia and tritanopia (all ≥ 7). Every focus-ring background still has at least one ring band ≥ 3:1: white on dark backgrounds, black on light ones.

6.3 Trade-offs to be aware of
Google Fonts is loaded from fonts.googleapis.com, which sends visitors' IP addresses to Google. For privacy (e.g. GDPR) or offline use, self-host the font files instead. If the font can't load, the page falls back to the system sans-serif and stays fully readable.
There is no manual light/dark toggle. The page follows the operating-system setting. A toggle would be a small JS addition if wanted.
