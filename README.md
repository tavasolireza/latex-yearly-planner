# LaTeX Yearly Planner

Digital planner for e-ink devices including **Remarkable Paper Pro**, Remarkable 2, Supernote, and Kindle Scribe.

This fork adds support for **Remarkable Paper Pro** (11.8" display).

---

## Pre-generated Planners

If you just want to download a planner without building, check the [Discussions](https://github.com/kudrykv/latex-yearly-planner/discussions) on the original repo for pre-generated PDFs.

---

## Remarkable Paper Pro Configuration

This fork adds configuration files for the **Remarkable Paper Pro** (179mm × 239mm display).

### Paper Pro Settings

| Setting | Value |
|---------|-------|
| Paper size | 17.9cm × 23.9cm |
| Time format | AM/PM |
| Schedule hours | 8am to 11pm |
| Style | Lined |
| Week start | Monday |
| Daily calendar | Yes (mini calendar on daily pages) |
| Template | Months on Side (MOS) |

### New Config Files

- `cfg/rmpro.base.yaml` - Paper Pro dimensions and layout
- `cfg/rmpro.mos.default.yaml` - MOS template settings
- `cfg/rmpro.mos.default.dailycal.yaml` - Daily calendar settings

---

## Prerequisites (macOS)

Install Go and LaTeX:

```bash
brew install go
brew install --cask mactex
```

After installing MacTeX, restart your terminal or run:
```bash
eval "$(/usr/libexec/path_helper)"
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install golang texlive-full
```

---

## Build Instructions

### Clone the Repository

```bash
git clone https://github.com/tavasolireza/latex-yearly-planner.git
cd latex-yearly-planner
```

### Build for Remarkable Paper Pro

#### 2025

```bash
export PLANNER_YEAR=2025
go run cmd/plannergen/plannergen.go --config "cfg/base.yaml,cfg/rmpro.base.yaml,cfg/template_months_on_side.yaml,cfg/rmpro.mos.default.yaml,cfg/rmpro.mos.default.dailycal.yaml"
cd out
xelatex rmpro.mos.default.dailycal.tex
xelatex rmpro.mos.default.dailycal.tex
```

**Important:** Run `xelatex` **twice** - this is required for proper navigation links!

#### 2026

```bash
cd ..
export PLANNER_YEAR=2026
go run cmd/plannergen/plannergen.go --config "cfg/base.yaml,cfg/rmpro.base.yaml,cfg/template_months_on_side.yaml,cfg/rmpro.mos.default.yaml,cfg/rmpro.mos.default.dailycal.yaml"
cd out
xelatex rmpro.mos.default.dailycal.tex
xelatex rmpro.mos.default.dailycal.tex
```

### Build for Other Devices

#### Remarkable 2

```bash
export PLANNER_YEAR=2025
go run cmd/plannergen/plannergen.go --config "cfg/base.yaml,cfg/rm2.base.yaml,cfg/template_months_on_side.yaml,cfg/rm2.mos.default.yaml,cfg/rm2.mos.default.dailycal.yaml"
cd out
xelatex rm2.mos.default.dailycal.tex
xelatex rm2.mos.default.dailycal.tex
```

#### Supernote A5X

```bash
export PLANNER_YEAR=2025
go run cmd/plannergen/plannergen.go --config "cfg/base.yaml,cfg/template_months_on_side.yaml,cfg/sn_a5x.mos.default.yaml,cfg/sn_a5x.mos.default.dailycal.yaml"
cd out
xelatex sn_a5x.mos.default.dailycal.tex
xelatex sn_a5x.mos.default.dailycal.tex
```

### Output

The generated PDF will be in the `out/` folder. Rename it to something meaningful like:
- `rmpro.mos.lined.ampm.dailycal.2025.pdf`

---

## Customization Guide

When customizing, you must change settings in **all three** device-specific config files to ensure consistency. For Paper Pro:

1. `cfg/rmpro.base.yaml`
2. `cfg/rmpro.mos.default.yaml`
3. `cfg/rmpro.mos.default.dailycal.yaml`

### Changing Schedule Hours

Edit in all three device config files:

```yaml
layout:
  numbers:
    dailybottomhour: 8    # Start hour (8 = 8am)
    dailytophour: 23      # End hour (23 = 11pm)
```

**Examples:**
- 8am to 11pm: `dailybottomhour: 8`, `dailytophour: 23`
- 7am to 10pm: `dailybottomhour: 7`, `dailytophour: 22`
- 9am to 9pm: `dailybottomhour: 9`, `dailytophour: 21`

**Note:** More hours = less space for the mini calendar. If the layout overflows to two pages, reduce the hour range.

### Changing Number of Lines

In the device base config (e.g., `cfg/rmpro.base.yaml`):

```yaml
layout:
  numbers:
    quarterlylines: 44      # Lines on quarterly overview pages
    weeklylines: 13         # Lines on weekly pages
    dailynotes: 32          # Note lines on daily pages (right side)
    dailytodos: 10          # Number of "Top priorities" checkboxes
    notesonpage: 44         # Lines on notes pages
    notesindexpages: 3      # Number of notes index pages
```

### Changing Time Format

In `cfg/base.yaml`:

```yaml
ampmtime: true    # true = AM/PM format, false = 24-hour format
```

### Changing Line Style

In `cfg/base.yaml`:

```yaml
dotted: false     # false = lines, true = dots
```

### Changing Week Start Day

In `cfg/base.yaml`:

```yaml
weekstart: 1      # 0 = Sunday, 1 = Monday
```

### Changing Page Margins

In the device base config (e.g., `cfg/rmpro.base.yaml`):

```yaml
layout:
  paper:
    margin:
      top: .3cm
      bottom: 0.6cm
      left: 1.4cm
      right: 0.3cm
```

---

## Troubleshooting

### Missing LaTeX packages

MacTeX should auto-install missing packages. If not:

```bash
sudo tlmgr install <package-name>
```

### Daily page overflows to two pages

The schedule has too many hours. Solutions:
- Reduce the hour range (`dailybottomhour` / `dailytophour`) in all three device config files
- Reduce `dailynotes` to give more space

### Calendar looks compressed or links don't work

Make sure you run `xelatex` **twice** for MOS templates.

### Changes not taking effect

Remember to change settings in **all three** device config files, then rebuild from the project root (not from `out/`).

---

## Credits

Original project by [kudrykv](https://github.com/kudrykv/latex-yearly-planner).
