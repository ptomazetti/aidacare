# Range Match

A product selection and comparison tool for Aidacare showroom staff, covering Mobility Aids, Manual Wheelchairs, Cushions and Bedroom.

It answers one question at the counter: **what does this client need, and what do we have that meets it.**

## What it does

**What the client needs.** Staff enter what they know about the client: weight, seat width, narrowest doorway, handle height, funding scheme, where the product will be used, transport, who propels the chair, whether the client can take weight through their hands and wrists, pressure injury risk and any budget ceiling. Anything left on "not a deciding factor" is ignored, so a half filled form still returns a usable shortlist.

Every product is then shown with a plain verdict:

- **Meets every requirement**
- **Short on preference** — it fails only on something that does not affect suitability, such as where it will be used or the budget
- **Not suitable** — it fails a requirement that does affect suitability: safe working load, seat width, doorway width, handle height, funding scheme, pressure risk rating, propulsion or forearm support

Each product lists the reason for every tick and cross, so staff can say why to the client rather than reading a number off a shelf.

**What the range does not cover.** Two different gaps are reported. One is a requirement nothing in the range meets at all. The other is harder to see and more common: every requirement has some product that meets it, but no single product meets them together, such as a 185 kg client through a 760 mm doorway. Either way the tool names the closest products and what each falls short by.

**Range gaps are logged, not just displayed.** Staff log the gap in one click and it carries the date, the showroom, the full client requirement, what the range could not meet and the closest product. Gaps build into a list with a status of open, reviewed, actioned or not a gap, and export as plain text for a range review. Staff can also report a specification that looks wrong from any product card, which is how a stale catalogue gets found.

Where the page is served with a shared database, gaps and reports from every showroom land in one list. Without one they are kept in that browser only, and the page says which is in force.

**Accessories.** Each product carries what it is usually sold with and why, including across categories, so a wheelchair brings up the cushion matched to the client's pressure risk rather than to the chair. One click adds a pairing to the comparison.

**What to say.** Every product carries an answer to "why does this one cost more", where it sits against the competition, and the limitation to state out loud. It is a selling tool as well as a selection tool.

**Photos.** One photo per product, shown in the results, the comparison and the Good Better Best matrix. On the published page an editor uploads them on the Catalogue tab and every viewer sees them.

**Compare.** Any number of products can be ticked and lined up side by side. Rows that differ are shaded, so the real choice is visible without reading every line. Stated limitations are shown, not left out.

**Full range.** The Good Better Best matrix in the agreed format: exactly three columns, rows are the customer need segments that genuinely separate a decision in that range, and each cell holds at most two models with a photo and up to three reason phrases written as `label - supporting number`. A model appears in more than one cell where it honestly serves more than one need. An empty position is left empty rather than filled to tidy the grid, and the count of empty positions is shown as a prompt, not a verdict. Header colours follow the Aidacare matrix standard.

The customer need rows shipped here are proposed, not approved. Change them with the `segments` column when you load the catalogue.

**In the showroom.** Display, demonstration readiness and stock for the selected showroom, with required products that are not on display called out.

**Catalogue.** Source and last checked date, CSV load of the real catalogue, and a plain text export of the client requirement, the shortlist and any range gaps.

## Printed reports

Four, all built into a dedicated print root so no page chrome reaches the paper:

- **Client report** — the client requirement, each recommended option with its key figures, what it falls short on, its stated limitation, its accessories and its funding, then what the range does not cover, then the side by side table. Carries a client reference and a prepared by name.
- **Comparison** — the requirement plus the side by side table.
- **Matrix** — one category's Good Better Best matrix in landscape, header colours preserved.
- **Gap list** — every logged range gap with its requirement and status.

While the catalogue is sample data, every report prints a red warning at the top saying so. Each report ends with the source, the last checked date, and a line stating that it does not replace a clinical assessment.

## Data

The tool ships with generated sample specifications so the matching, comparison and gap rules can be checked. These are labelled as sample on every screen and in the export. They are not the Aidacare catalogue.

Load the real catalogue on the Catalogue tab. The loader will not accept rows until a source and a last checked date are recorded, because a specification without a source cannot be quoted to a customer.

CSV columns:

```
sku,name,category,family,position,price,swl_kg,seat_width_mm,overall_width_mm,
handle_min_mm,handle_max_mm,product_weight_kg,folds,environment,propulsion,support,
pressure_risk,funding,customer_need,functional_difference,customer_value,limitations,
objection,competitor,accessories,range_status
```

(one line, wrapped here for reading)

Quoted fields are handled properly, so product copy containing commas is safe.

- `category` is `MA`, `MW`, `CU` or `BD`
- `position` is `Good`, `Better` or `Best`
- `pressure_risk` is `Low`, `Moderate`, `High`, or empty for a product that is not pressure care
- `environment` is `Indoor`, `Outdoor` or `Both`
- `propulsion` is `Self`, `Attendant` or empty
- `support` is `Hands`, `Forearms`, `Seated` or empty
- `range_status` is `Required`, `Optional` or `Local`
- `funding` is a semicolon separated list, for example `NDIS;DVA;Aged Care Package;Private`
- `accessories` is a semicolon separated list of `sku:reason`, for example `AID-9001:moderate pressure risk;AID-9002:high pressure risk`
- `overall_width_mm` drives the doorway check, `handle_min_mm` and `handle_max_mm` drive the handle height check
- `objection` answers why the product costs more than the tier below it; `competitor` is where it sits against the market

## Running it

One file, no build step and no dependencies. Open `index.html` in a browser, or serve the folder from any static host.

```
python3 -m http.server 8000
```

Self hosted, everything is kept in the browser's own storage: the client requirement, the comparison, the loaded catalogue, and the logged gaps and reports. They stay on that device.

Published as a Claude artifact with the `db`, `user` and `assets` capabilities, the gap log, the specification reports and the product photos are shared with everyone who can open the page, and only editors can change the photo set. The page checks at load and tells the user which of the two is in force.

## Limits

This tool does not replace a clinical assessment, a prescription or an occupational therapy recommendation. Safe working load, seat width and pressure care selection must be confirmed against current product documentation before an order is placed.
