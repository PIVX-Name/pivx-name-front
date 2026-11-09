# Image assets

All artwork derives from two SVG sources, so the icon set and the social card cannot drift
apart. Regenerate after editing either source — the PNGs are build output, not originals.

| Source | Used for |
|:---|:---|
| `../favicon.svg` | the full brand mark: shield + DNS-node overlay |
| `favicon-small.svg` | 16px and 32px only — same shield, overlay dropped and shield enlarged, because at 16px the overlay is ~5px across and reads as noise |
| `og-image.svg` | the 1200x630 social card |

The SVG lives only at `images/favicon.svg`, which is what both `index.html` and
`app.html` link. There is no root-level copy: browsers only auto-request `/favicon.ico`
(which does sit at the root) when a page declares no `<link rel="icon">`, and never
`/favicon.svg`. A root duplicate would just be a second file to keep in sync.

## Regenerating

```bash
cd ..                      # repository root

# full mark
inkscape favicon.svg -w 180 -h 180 -o images/apple-touch-icon.png
inkscape favicon.svg -w 192 -h 192 -o images/android-chrome-192x192.png
inkscape favicon.svg -w 512 -h 512 -o images/android-chrome-512x512.png

# simplified mark for the small sizes
inkscape images/favicon-small.svg -w 16 -h 16 -o images/favicon-16x16.png
inkscape images/favicon-small.svg -w 32 -h 32 -o images/favicon-32x32.png

# multi-resolution .ico: simplified at 16/32, full at 48
D=$(mktemp -d)
inkscape images/favicon-small.svg -w 16 -h 16 -o $D/i16.png
inkscape images/favicon-small.svg -w 32 -h 32 -o $D/i32.png
inkscape favicon.svg -w 48 -h 48 -o $D/i48.png
convert $D/i16.png $D/i32.png $D/i48.png favicon.ico && rm -rf $D

# social card
inkscape images/og-image.svg -w 1200 -h 630 -o images/og-image.png
convert images/og-image.png -strip PNG24:images/og-image.png
```

Brand colours, matching `css/style.css` and `tailwind.config.js`:
`#c084fc` -> `#7e22ce` gradient, `#a855f7` accent, `#0d0614` page, `#1a0b2e` raised,
`#d8b4fe` light accent.
