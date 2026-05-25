# NZ Wedding Guide

Astro site for wedding guests traveling to New Zealand.

## Local Development

Run commands from the repository root:

```sh
fnm use
npm install
npm run dev
```

Build for production:

```sh
fnm use
npm run build
```

## Project Structure

```text
/
├── public/
├── src/
│   ├── data/
│   ├── layouts/
│   ├── pages/
│   └── styles/
│       └── theme.css
├── astro.config.mjs
└── package.json
```

## Theme Palette

All site colors are centralized in:

- `src/styles/theme.css`

The base layout imports that file once, and all page/layout styles consume those CSS custom properties.

### Editing Colors

1. Open `src/styles/theme.css`.
1. Update token values using hex codes.
1. Save and refresh the site.

The current theme uses standard hex and hex-with-alpha (`#RRGGBBAA`) values.

Examples:

- `#1d5f5a` (solid color)
- `#ffffffb3` (color with alpha)

### Token Groups

- Base surfaces/text: `--bg`, `--surface`, `--surface-strong`, `--text`, `--muted`
- Brand accents: `--accent`, `--accent-soft`, `--accent-warm`
- Borders and lines: `--line`, `--panel-line`, `--prose-line`, `--nav-hover-line`, `--card-hover-line`
- Effects and overlays: `--shadow`, `--card-hover-shadow`, `--bg-glow-warm`, `--hero-aside-overlay`
- Hero-specific tones: `--hero-aside-gradient-start`, `--hero-aside-gradient-end`, `--hero-aside-text`, `--hero-aside-muted`

### Notes

- Keep token names stable and only change values for safer visual updates.
- Asset files (for example `public/favicon.svg`) are outside the CSS theme system.
