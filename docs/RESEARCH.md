# RESEARCH — Oráculo Light

Engineering notes for the **light** sibling of Oráculo.
Verified on Omarchy **4.0.6-1** (2026-10-22). The dark sibling's
[RESEARCH.md](https://github.com/PedroAugustoOK/omarchy-oraculo-theme/blob/main/RESEARCH.md)
documents the shared staging flow, template list, deny list and asset
pipeline; this file records what is light-specific and how the pair was
verified together.

## 1. Light-mode conventions, read from the stock themes

Studied on disk: `/usr/share/omarchy/themes/catppuccin-latte/colors.toml`,
`flexoki-light`, `lupine`, `rose-pine` and `white`.

| Convention | Stock behavior | Oráculo Light |
|---|---|---|
| `mode` | `"light"` | `"light"` (explicit; no `light.mode` side file) |
| `foreground` | dark ink on paper | `#1C2B3A` |
| `bright_foreground` | equals or tops the neutral ramp | `#1C2B3A`, tied with `foreground` |
| `light_foreground` | slightly lighter secondary text | `#334A60` |
| `dark_foreground` | low-contrast chrome text | `#55677B` (5.40:1) |
| `dark_background` / `darker_background` | surfaces recede to darker tones | `#E8EFF5` / `#DCE6EF` |
| `lighter_background` | a subtle raised highlight | `#FBFDFF` |
| ANSI `bright_*` | same as base or a close, readable sibling | base floor 5.39:1; bright floor 4.51:1 on paper |
| `accent` | one saturated accent, darkened for paper | `#2B6A96` (the dark twin uses its sky-steel accent) |

The stock light themes do not hold every ANSI entry to 4.5:1; some bright
colours sit in the low 3s. Oráculo Light tightens that rule: the base ANSI
floor is 5.39:1 on paper and the bright floor is 4.51:1. The only sub-4.5
entries are chrome roles (`muted` 2.02:1, a hairline), the selection fill
(1.19:1), and bright ANSI on the darker chrome surfaces, where it is not used
for body text.

## 2. Measured contrast

Against `background #F3F7FA` (WCAG 2.x relative-luminance formula, the same
one used by `omarchy dev theme-preview`):

| Role | Key | Hex | Ratio |
|---|---|---|---|
| main text | `foreground` / `bright_foreground` | `#1C2B3A` | **13.39:1** |
| emphasised text | `light_foreground` | `#334A60` | **8.52:1** |
| chrome text | `dark_foreground` | `#55677B` | **5.40:1** |
| accent / blue | `accent` / `blue` | `#2B6A96` | **5.41:1** |
| hairline / chrome | `muted` | `#9FB2C4` | 2.02:1 |
| selection fill | `selection` | `#D7E5F1` | 1.19:1 |
| selected text | `foreground` on `selection` | — | **11.24:1** |

Full ANSI set:

| Base | Ratio | Bright | Ratio |
|---|---|---|---|
| `red #B03A48` | 5.50:1 | `bright_red #C24550` | 4.58:1 |
| `yellow #7A6214` | 5.44:1 | `bright_yellow #876F1B` | 4.51:1 |
| `orange #9A522A` | 5.39:1 | — | — |
| `green #2F6B47` | 5.88:1 | `bright_green #387A52` | 4.79:1 |
| `cyan #177076` | 5.39:1 | `bright_cyan #1C7C82` | 4.58:1 |
| `blue #2B6A96` | 5.41:1 | `bright_blue #2F76A8` | 4.56:1 |
| `magenta #9A3F86` | 5.69:1 | `bright_magenta #A94D95` | 4.66:1 |
| `brown #6B4630` | 7.66:1 | — | — |

The base ANSI floor is `orange`/`cyan` at 5.39:1; the bright floor is
`bright_yellow` at 4.51:1. `bright_blue` is now 1.19:1 away from its base
`blue`, so the two remain distinguishable and the old near-identical warning
is gone.

### Surface-aware measurements

The four opaque surfaces are `background #F3F7FA`, `dark_background #E8EFF5`,
`darker_background #DCE6EF` and `lighter_background #FBFDFF`; `selection` is a
fifth, non-text fill.

| Role group | Background | Dark background | Darker background | Selection fill |
|---|---|---|---|---|
| Base text roles + base ANSI (minimum) | 5.39:1 | 5.00:1 | 4.59:1 | 4.53:1 |
| `dark_foreground` | 5.40:1 | 5.01:1 | 4.60:1 | 4.53:1 |
| `accent` / `blue` | 5.41:1 | 5.02:1 | 4.61:1 | 4.54:1 |
| Bright ANSI range | 4.51–4.79:1 | 4.19–4.44:1 | 3.84–4.08:1 | 3.79–4.02:1 |

The base text/ANSI group clears 4.5:1 everywhere, including the selection fill.
Selected text itself always uses `foreground`, which is 11.24:1 on the
selection. Bright ANSI is guaranteed only on the paper background; it is
terminal/app content, not chrome text, and the theme does not claim otherwise.

The surface ramp recedes in the expected light direction:
`darker_background` → `dark_background` → `background`, with
`lighter_background` as a subtle highlight band above `background`. Adjacent
surfaces are intentionally close; a light theme should not invert its ramp.

## 3. Relationship to the dark sibling

Both themes share the same construction rules, not just a hue swap:

- the same 26-key contract and the same optional border-gradient keys;
- the same accent role: focused borders, active bar elements, selections,
  links;
- the same border geometry and angle, with the light accent moved from
  `#5FA8D3` to `#2B6A96` and the second stop moved to the deep steel
  `#174A6D`;
- the same sigil geometry for `unlock.png` and `preview-unlock.png`;
- the same three-wallpaper composition (bloom, recess, diagonal wash) rendered
  in the light palette;
- the same install slug pattern, `omarchy-oraculo-light-theme` →
  `oraculo-light`.

The dark theme is the night reading of the same system; the light theme is the
paper reading. Neither is a simple inversion of the other. The light paper is
tinted sky, not pure white, and the accent is darkened because the same
saturation that reads as calm on blue-black vibrates on paper.

## 4. Resolution and staging

- `omarchy-theme-color --file colors.toml --all` resolves **58 keys** on
  4.0.6-1, including `mode = light`, `theme_type = light`, the aliases,
  `color0`–`color15`, `purple` → the magenta value, and both border gradient
  strings intact.
- Headless staging (`omarchy theme set oraculo-light`) produces the full theme
  tree, with no ignored-file warnings and no unresolved `{{ ... }}`
  placeholders in generated text configs. `omarchy theme current` reports
  `Oraculo Light`. The staged source RESEARCH file contains a literal
  `{{ ... }}` example; it is documentation, not an unresolved template.
- A clean install from the public URL is exercised in an isolated `HOME`;
  `omarchy theme install https://github.com/PedroAugustoOK/omarchy-oraculo-light-theme`
  clones, stages and applies with no ignored-file warnings and zero unresolved
  placeholders in the text configs.
- The dark theme remains installed; the final active state is **Oraculo
  Light** with the **Oraculo Light Mist** background.
- The repository ships no denied files: no `*.lua`, no `alacritty.toml`,
  `foot.ini`, `ghostty.conf`, `kitty.conf`, `vscode.json`, no symlinks.

## 5. Assets

- **Wallpapers:** Mist, Linen and Haze, native **3840×2160** truecolor PNG,
  about 2.1 MB each. Generated by `scripts/make-wallpapers.sh` from the
  palette; the same soft-wash language as the dark set, with sky instead of
  night. A fixed seed and fine grain keep the render deterministic and avoid
  banding.
- **Preview:** 1800×1012, a static artwork included with the theme. An original
  neo-expressionist composition inspired by Basquiat's visual language:
  crown, rough anatomical figure, medical and maritime motifs, paint marks,
  marginalia and a D20 on paper. It is not generated at build time and is not
  a desktop screenshot.
- **Lock art:** the Oráculo sigil in the light accent. `unlock.png` is
  1108×523 transparent (the Tokyo Night canvas); `preview-unlock.png` is
  1920×1080 and places the sigil on the stock preview canvas with the theme's
  own lock glyph and password field. The light version replaces the dark
  theme's glow with a softer blue bloom at the same opacity profile.
- **Icons / keyboard:** `Yaru-blue`; keyboard backlight `2b6a96` — the theme's
  own accent, which is also what the built-in keyboard template generates.

## 6. Reproducibility and claim checks

- The wallpaper and lock scripts are seeded or deterministic; the lock art
  reproduces **pixel-identical** (`compare -metric AE` = 0) and the wallpaper
  seed is fixed. The preview is a static asset. The generated assets were
  regenerated from the committed palette after the last colour adjustment.
- `scripts/check-theme.py` reports no blocking problems. Its notes record the
  selection-fill exception and the bright-ANSI chrome limitation; the
  `bright_blue` separation warning is gone (1.19:1 from the base).
- Every hex code and contrast ratio in `README.md` was checked against
  `colors.toml` with `OmarchyOpenCode/tools/check_readme.py`.
- The light README's claims about the ANSI floors and the selection pairing are
  measured, not copied from the dark theme.
- CI runs the same palette/contrast/asset audit, shell syntax checks and lock
  render on every push and pull request.

## 7. Limits

- The preview is curated artwork, not a live desktop screenshot.
- Plymouth is optional and machine-local; the theme ships the art but does not
  change the boot screen on install.
- No animated intros (OWE is optional and the theme stays static).
- Bright ANSI is guaranteed on the paper field but not on the darker chrome
  surfaces; this is a documented terminal-content exception.
- Static palette and template checks do not prove every application's rendered
  appearance; they do prove that all Omarchy-generated apps receive the same
  palette.
- The display name is `Oraculo Light` (slug-derived), without the accent.

## 8. Checklist

- [x] `mode = "light"` and all 26 canonical keys explicit.
- [x] Base ANSI at or above 4.5:1 on every opaque surface and on the selection
  fill; bright ANSI at or above 4.5:1 on the paper background.
- [x] `selection` and `muted` documented as fill/chrome roles.
- [x] Three native 4K wallpapers, flat directory, about 2.1 MB each.
- [x] `preview.png`, `unlock.png`, `preview-unlock.png`.
- [x] `icons.theme`, `keyboard.rgb`, README, RESEARCH, LICENSE, `.gitignore`.
- [x] No denied files or symlinks.
- [x] Seeded wallpaper and deterministic lock scripts; static preview.
- [x] CI workflow and surface-aware `scripts/check-theme.py`.
