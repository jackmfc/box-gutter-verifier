# Curved Box Gutter — NCC Performance Solution Verifier

A **self-contained, standalone** browser tool that verifies a box gutter curved in plan as a
**Performance Solution** under the NCC, using AS/NZS 3500.3 stormwater hydraulics. It maps the
departure onto the correct NCC Performance Requirement by building classification, checks the
curvature (superelevation) and overflow conditions, and produces a branded Performance
Solution-structured PDF report.

This project is **completely independent** — it shares no code, server, or data with any other
application.

## Run it

Just open `index.html` in any browser. No build, no server, no backend required.

To host it (e.g. on a static host like Netlify, GitHub Pages, S3, or any web server), upload the
three files together:

```
index.html    the app
logo.js       branded logo for the PDF header (optional — falls back to text)
README.md     this file
```

Or serve locally:

```
npx http-server .          # then open the printed URL
# or: python3 -m http.server
```

(jsPDF is loaded from a CDN the first time, then cached. If it can't load, the PDF falls back to the
browser's print-to-PDF.)

## Rainfall input

Enter the **5-minute, 1% AEP** design intensity directly, or look it up at the
[BoM IFD2016 tool](http://www.bom.gov.au/water/designRainfalls/revised-ifd/) (linked in-app) and type
it in.

**Optional live auto-fill:** the tool can auto-fill intensities from an address *if* you point it at a
backend service that exposes `POST /api/hydraulics/ifd-by-address` (returning
`{ location, ifd: {'10%':N,...}, bomToolUrl }`). Paste that service's base URL under
**Optional BoM lookup service (advanced)**. Left blank (the default), the tool is fully static and
needs no network beyond the BoM deep link.

## What it does

Pick **governing NCC edition** (2022 or 2025), **building class**, and **state**:

- Class 1 & 10 → **Volume Two** (Part H2, `H2P1`); Class 2–9 → **Volume One** (Part F1).
- Auto-loads the **relevant Performance Requirement(s)** with verbatim NCC wording, and (default)
  cross-references the **equivalent under the other edition** for the 2022↔2025 transition.
- Identifies the **DtS provision departed from** (AS/NZS 3500.3 via F1D3 / H2D6) and lets you declare
  the **Assessment Method(s)** used (NCC Governing Requirement **A2G2**).
- Flags the **Victorian** variation (Housing Provisions Part 7.4 excludes box gutters).

### Checks

1. **Curvature (superelevation)** — Δh = V²·b/(g·R); confirms outer-wall freeboard is maintained so
   the 1% AEP storm does not enter the building.
2. **Overflow provision** (AS/NZS 3500.3 §3.5, primary outlet assumed blocked) — selectable hydraulic
   control: **weir** `H=(Q/Cw·L)^⅔` or **orifice** `H=(Q/Cd·A)²/2g`.
3. **Flow-regime flags** — supercritical (Fr > 1) and below-critical depth warnings.

Design flow uses the Rational Method `Q = C·i·A/3600`.

## Important

This is a calculation aid supporting a Performance Solution. NCC clause references are reproduced from
ncc.abcb.gov.au and must be confirmed against the edition in force for the project and any state
variations. The overflow heads are first-principles weir/orifice idealisations, and the 2022↔2025
edition pairing is an analytical equivalence by criterion (not an official NCC concordance) — confirm
both before issuing. Outputs do not constitute regulatory approval.

`g = 9.80665 m/s²`.
