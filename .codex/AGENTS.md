# Archive Notice Banner Pattern

Use this pattern when adding a persistent archive or maintenance notice to a static web map or similar fixed-layout app.

## Goal

Add a top-of-page notice banner that:

- clearly communicates archived status
- remains visible above the app chrome
- meets basic accessibility expectations
- does not break the positioning of existing fixed or absolute UI controls

## Implementation Notes

### HTML

- Insert the banner immediately inside `<body>`.
- Keep the content simple:
  - a short title
  - a short explanatory sentence
  - a link to the request/help form
- Prefer descriptive link text over showing the raw URL.
- Expect final link wording to affect wrapping and banner height.
- Use `role="status"` and `aria-live="polite"` only if the banner is intended to be announced as status content.
- Mark decorative icon content with `aria-hidden="true"`.

Recommended structure:

```html
<div id="archive-banner" role="status" aria-live="polite">
  <div class="archive-banner-icon" aria-hidden="true">i</div>
  <div class="archive-banner-content">
    <p class="archive-banner-title">This page has been archived and is not updated</p>
    <p class="archive-banner-text">
      If you need assistance with this page,
      <a href="..." target="_blank" rel="noopener noreferrer">please complete the request for accessible web content form</a>.
    </p>
  </div>
</div>
```

Preferred link and wording for AGSL archive projects:

```html
<p class="archive-banner-text">
  If you need assistance with this page,
  <a href="https://uwm.edu/libraries/digital-collections/digital-collections-request-for-accessible-web-content-form/" target="_blank" rel="noopener noreferrer">please complete the request for accessible web content form</a>.
</p>
```

### CSS

- Style the banner as `position: fixed` at the top of the page.
- Use a CSS custom property for banner height, for example `--archive-banner-height`.
- Treat `--archive-banner-height` as a layout token to tune after the final banner copy, text sizing, and padding are in place.
- Apply the top offset to `body`, not to `html`.
- Size the main app area with `100vh` math when needed, not `100%`, to avoid bottom whitespace after adding the banner.
- Shift fixed-position app chrome down using that variable.
- Prefer adjusting top-level control containers once instead of offsetting child controls individually.
- Avoid decorative top borders or extra strips if they create visual spacing issues.

Recommended accessibility choices:

- banner background should be light enough for strong text contrast
- link color must pass contrast on the banner background
- icon foreground and icon background must also pass contrast
- allow long URLs to wrap with `word-break: break-word`

Preferred visual treatment from this project:

```css
html {
  --archive-banner-height: 58px;
}

body {
  margin: 0;
  padding-top: var(--archive-banner-height);
}

main {
  min-height: calc(100vh - var(--archive-banner-height));
  height: calc(100vh - var(--archive-banner-height));
}

#archive-banner {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 2000;
  display: flex;
  align-items: flex-start;
  gap: 1em;
  padding: 0.75em 1.25em 0.7em;
  background: #d6e5ea;
  border-bottom: 1px solid #9eb8c2;
  color: #1c1c1c;
  font-size: 16px;
  line-height: 1.4;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.archive-banner-icon {
  flex: 0 0 auto;
  width: 1.75em;
  height: 1.75em;
  border-radius: 50%;
  background: #005a7a;
  color: #ffffff;
  font-size: 14px;
  font-weight: bold;
  line-height: 1.75em;
  text-align: center;
}

.archive-banner-content {
  min-width: 0;
}

.archive-banner-title,
.archive-banner-text {
  margin: 0;
}

.archive-banner-title {
  font-size: 1.05em;
  font-weight: bold;
}

.archive-banner-text {
  margin-top: 0.2em;
}

.archive-banner-text a {
  color: #005073;
  font-weight: bold;
  word-break: break-word;
}

@media screen and (max-width: 767px) {
  html {
    --archive-banner-height: 92px;
  }

  #archive-banner {
    padding: 0.9em 1em;
  }

  .archive-banner-title {
    font-size: 1em;
  }

  .archive-banner-text {
    font-size: 0.95em;
  }
}
```

### Layout Best Practices

When the application already has fixed UI like headers, top buttons, map controls, sliders, and search boxes:

- move the shared header and top controls down by `var(--archive-banner-height)`
- move the main map control container down if needed
- do not also shift nested controls if their parent is already offset
- if the application fills the screen, update the main app container height with `calc(100vh - var(--archive-banner-height))`
- do not put the archive offset on both `html` and `body`, or you may introduce extra top and bottom whitespace

Important lesson from this project:

- the search bar was misaligned because both `.leaflet-top` and `.geocoder-control` were offset
- the correct fix was to offset the parent control container and leave `.geocoder-control` at its original `top` values
- the bottom gap was caused by mixing a `body` top offset with `100%`-based height math; switching the app container to `100vh` resolved it

### Mobile

- define a larger `--archive-banner-height` in the mobile media query if the banner wraps to two lines
- reduce title/text size slightly on small screens
- re-check banner height after changing link wording, since descriptive link text may wrap differently than a raw URL
- verify that dropdowns, search controls, and other fixed map UI still fit below the banner

## Checklist

- Banner is inserted immediately inside `<body>`
- Banner text is readable and concise
- Link contrast passes
- Icon contrast passes
- Long URL wraps safely
- `padding-top` is applied only to `body`
- full-height app containers use `100vh` math if needed
- Fixed headers/buttons are shifted below the banner
- Search and map controls are not double-offset
- Mobile layout is checked separately

## Reuse Guidance

For future projects, ask Codex to:

1. add a fixed accessible archive notice banner at the top of the page
2. use a CSS variable for banner height
3. apply the archive offset to `body`, not to `html`
4. use `100vh`-based layout math for full-height apps to avoid bottom whitespace
5. shift only the necessary top-level fixed layout elements
6. verify color contrast for text, links, and icons
7. avoid double-offsetting nested controls such as Leaflet geocoder widgets
