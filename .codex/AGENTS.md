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
- Use `role="status"` and `aria-live="polite"` only if the banner is intended to be announced as status content.
- Mark decorative icon content with `aria-hidden="true"`.

Recommended structure:

```html
<div id="archive-banner" role="status" aria-live="polite">
  <div class="archive-banner-icon" aria-hidden="true">i</div>
  <div class="archive-banner-content">
    <p class="archive-banner-title">This page has been archived and is not updated</p>
    <p class="archive-banner-text">
      If you need assistance with this page, please use this form to make a request:
      <a href="...">...</a>
    </p>
  </div>
</div>
```

### CSS

- Style the banner as `position: fixed` at the top of the page.
- Use a CSS custom property for banner height, for example `--archive-banner-height`.
- Shift fixed-position app chrome down using that variable.
- Prefer adjusting top-level control containers once instead of offsetting child controls individually.
- Avoid decorative top borders or extra strips if they create visual spacing issues.

Recommended accessibility choices:

- banner background should be light enough for strong text contrast
- link color must pass contrast on the banner background
- icon foreground and icon background must also pass contrast
- allow long URLs to wrap with `word-break: break-word`

### Layout Best Practices

When the application already has fixed UI like headers, top buttons, map controls, sliders, and search boxes:

- move the shared header and top controls down by `var(--archive-banner-height)`
- move the main map control container down if needed
- do not also shift nested controls if their parent is already offset

Important lesson from this project:

- the search bar was misaligned because both `.leaflet-top` and `.geocoder-control` were offset
- the correct fix was to offset the parent control container and leave `.geocoder-control` at its original `top` values

### Mobile

- define a larger `--archive-banner-height` in the mobile media query if the banner wraps to two lines
- reduce title/text size slightly on small screens
- verify that dropdowns, search controls, and other fixed map UI still fit below the banner

## Checklist

- Banner is inserted immediately inside `<body>`
- Banner text is readable and concise
- Link contrast passes
- Icon contrast passes
- Long URL wraps safely
- Fixed headers/buttons are shifted below the banner
- Search and map controls are not double-offset
- Mobile layout is checked separately

## Reuse Guidance

For future projects, ask Codex to:

1. add a fixed accessible archive notice banner at the top of the page
2. use a CSS variable for banner height
3. shift only the necessary top-level fixed layout elements
4. verify color contrast for text, links, and icons
5. avoid double-offsetting nested controls such as Leaflet geocoder widgets
