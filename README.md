# Range Match

A product selection and comparison tool for Aidacare showroom staff, covering Mobility Aids, Manual Wheelchairs, Cushions and Bedroom.

It answers one question at the counter: **what does this client need, and what do we have that meets it.**

## What it does

**What the client needs.** Staff enter what they know about the client: weight, seat width, where the product will be used, transport, who propels the chair, whether the client can take weight through their hands and wrists, pressure injury risk and any budget ceiling. Anything left on "not a deciding factor" is ignored, so a half filled form still returns a usable shortlist.

Every product is then shown with a plain verdict:

- **Meets every requirement**
- **Short on preference** — it fails only on something that does not affect suitability, such as where it will be used or the budget
- **Not suitable** — it fails a requirement that does affect suitability: safe working load, seat width, pressure risk rating, propulsion or forearm support

Each product lists the reason for every tick and cross, so staff can say why to the client rather than reading a number off a shelf.

**What the range does not cover.** Where nothing in scope meets a requirement, the tool says so directly, names the closest product and what it falls short by, and prompts staff to record the gap. This is the part that separates a range gap from a selection problem.

**Compare.** Any number of products can be ticked and lined up side by side. Rows that differ are shaded, so the real choice is visible without reading every line. Stated limitations are shown, not left out.

**Full range.** The controlled Good Better Best matrix by product family. Each position carries the customer or clinical need, the functional difference and the customer value. Price alone never sets a position.

**In the showroom.** Display, demonstration readiness and stock for the selected showroom, with required products that are not on display called out.

**Catalogue.** Source and last checked date, CSV load of the real catalogue, and a plain text export of the client requirement, the shortlist and any range gaps.

## Data

The tool ships with generated sample specifications so the matching, comparison and gap rules can be checked. These are labelled as sample on every screen and in the export. They are not the Aidacare catalogue.

Load the real catalogue on the Catalogue tab. The loader will not accept rows until a source and a last checked date are recorded, because a specification without a source cannot be quoted to a customer.

CSV columns:

```
sku,name,category,family,position,price,swl_kg,seat_width_mm,product_weight_kg,folds,environment,propulsion,support,pressure_risk,customer_need,functional_difference,customer_value,limitations,range_status
```

- `category` is `MA`, `MW`, `CU` or `BD`
- `position` is `Good`, `Better` or `Best`
- `pressure_risk` is `Low`, `Moderate`, `High`, or empty for a product that is not pressure care
- `environment` is `Indoor`, `Outdoor` or `Both`
- `propulsion` is `Self`, `Attendant` or empty
- `support` is `Hands`, `Forearms`, `Seated` or empty
- `range_status` is `Required`, `Optional` or `Local`

## Running it

One file, no build step and no dependencies. Open `index.html` in a browser, or serve the folder from any static host.

```
python3 -m http.server 8000
```

The client requirement, the comparison selection and the loaded catalogue are kept in the browser's own storage. They stay on that device and do not reach another person or another server.

## Limits

This tool does not replace a clinical assessment, a prescription or an occupational therapy recommendation. Safe working load, seat width and pressure care selection must be confirmed against current product documentation before an order is placed.
