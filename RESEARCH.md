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
| `dark_foreground` | low-contrast chrome text | `#5E7389` (4.55:1) |
| `dark_background` / `darker_background` | surfaces recede to darker tones | `#E8EFF5` / `#DCE6EF` |
| `lighter_background` | a subtle raised highlight | `#FBFDFF` |
| ANSI `bright_*` | same as base or a close, readable sibling | all base and bright entries clear 4.5:1 |
| `accent` | one saturated accent, darkened for paper | `#2C6E9B` (the dark twin uses its sky-steel accent) |

The stock light themes do not hold every ANSI entry to 4.5:1; some bright
colours sit in the low 3s. Oráculo Light tightens that rule: the base ANSI
floor is 4.97:1 and the bright floor is 4.51:1. The only sub-4.5 entries are
the chrome roles `muted` (2.02:1, a hairline) and `dark_foreground` (4.55:1,
which clears it), plus `selection` (1.19:1, a fill rather than text).

## 2. Measured contrast

Against `background #F3F7FA` (WCAG 2.x relative-luminance formula, the same
one used by `omarchy dev theme-preview`):

| Role | Key | Hex | Ratio |
|---|---|---|---|
| main text | `foreground` / `bright_foreground` | `#1C2B3A` | **13.39:1** |
| emphasised text | `light_foreground` | `#334A60` | **8.52:1** |
| chrome text | `dark_foreground` | `#5E7389` | 4.55:1 |
| accent / blue | `accent` / `blue` | `#2C6E9B` | **5.12:1** |
| hairline / chrome | `muted` | `#9FB2C4` | 2.02:1 |
| selection fill | `selection` | `#D7E5F1` | 1.19:1 |
| selected text | `foreground` on `selection` | — | **11.24:1** |

Full ANSI set:

| Base | Ratio | Bright | Ratio |
|---|---|---|---|
| `red #B03A48` | 5.50:1 | `bright_red #C24550` | 4.58:1 |
| `yellow #7C6414` | 5.28:1 | `bright_yellow #876F1B` | 4.51:1 |
| `orange #A3562C` | 4.97:1 | — | — |
| `green #2F6B47` | 5.88:1 | `bright_green #387A52` | 4.79:1 |
| `cyan #177076` | 5.39:1 | `bright_cyan #1C7C82` | 4.58:1 |
| `blue #2C6E9B` | 5.12:1 | `bright_blue #2F76A8` | 4.56:1 |
| `magenta #9A3F86` | 5.69:1 | `bright_magenta #A94D95` | 4.66:1 |
| `brown #6B4630` | 7.66:1 | — | — |

The bright floor is `bright_yellow` at 4.51:1. `bright_blue` is only 1.12:1
away from its base colour; that is the cost of keeping both readable on paper.
The stock light themes make the same trade-off (several ship bright and base
identical), so the audit warning is accepted rather than hidden.

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
  `#5FA8D3` to `#2C6E9B` and the second stop moved to the deep steel
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
- Headless staging (`omarchy theme set oraculo-light`) produced 28 files, no
  ignored-file warnings, and no unresolved `{{ ... }}` placeholders in any
  text config. `omarchy theme current` reported `Oraculo Light`.
- A clean install from the public URL was exercised in an isolated `HOME`;
  `omarchy theme install https://github.com/PedroAugustoOK/omarchy-oraculo-light-theme`
  cloned, staged and applied with no ignored-file warnings and zero unresolved
  placeholders in the text configs.
- The dark theme was restored afterwards; `theme.name` returned to `oraculo`
  and the active background stayed `Oraculo Veil`.
- The repository ships no denied files: no `*.lua`, no `alacritty.toml`,
  `foot.ini`, `ghostty.conf`, `kitty.conf`, `vscode.json`, no symlinks.

## 5. Assets

- **Wallpapers:** Mist, Linen and Haze, 2560×1440 PNG, 54–72 KB. Generated by
  `scripts/make-wallpapers.sh` from the palette; the same soft-wash language
  as the dark set, with sky instead of night.
- **Preview:** 1800×1012, generated by `scripts/make-preview.sh`. Paper field,
  Oráculo Light wordmark, accent rule and ANSI swatches.
- **Lock art:** the Oráculo sigil in the light accent. `unlock.png` is
  1108×523 transparent; `preview-unlock.png` is 1920×1080 with the standard
  password-field geometry. The light version replaces the dark theme's glow
  with a softer blue bloom at the same opacity profile.
- **Icons / keyboard:** `Yaru-blue`; keyboard backlight `2c6e9b` — the theme's
  own accent, which is also what the built-in keyboard template generates.

## 6. Reproducibility and claim checks

- Re-running all three asset scripts in a scratch copy reproduced the preview,
  the lock art and all three wallpapers **pixel-identical** to the committed
  files (`compare -metric AE` = 0).
- The project audit reports **no blocking problems**. One warning remains:
  `bright_blue` is close to `blue`; see §2.
- Every hex code and contrast ratio in `README.md` was checked against
  `colors.toml` with `OmarchyOpenCode/tools/check_readme.py`; all pass.
- The light README's claims about the ANSI floors and the selection pairing are
  measured, not copied from the dark theme.

## 7. Limits

- The preview is original generated artwork, not a live desktop screenshot.
- Plymouth is optional and machine-local; the theme ships the art but does not
  change the boot screen on install.
- No animated intros (OWE is optional and the theme stays static).
- Static palette and template checks do not prove every application's rendered
  appearance; they do prove that all Omarchy-generated apps receive the same
  palette.
- The display name is `Oraculo Light` (slug-derived), without the accent.

## 8. Checklist

- [x] `mode = "light"` and all 26 canonical keys explicit.
- [x] All ANSI base and bright colours at or above 4.5:1.
- [x] `selection` and `muted` documented as fill/chrome roles.
- [x] Three 2560×1440 wallpapers, flat directory, under 3 MB each.
- [x] `preview.png`, `unlock.png`, `preview-unlock.png`.
- [x] `icons.theme`, `keyboard.rgb`, README, RESEARCH, LICENSE, `.gitignore`.
- [x] No denied files or symlinks.
- [x] Asset scripts reproduce the committed images.
