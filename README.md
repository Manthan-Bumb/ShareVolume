# Overview

This is a small, client-side web app that fetches SEC XBRL Company Concept data for EntityCommonStockSharesOutstanding and displays the highest and lowest values since FY 2021.

It:
- Loads AT&T (CIK 0000732717) by default.
- Filters .units.shares[] entries to fy > 2020 and numeric val.
- Computes max and min by val.
- Dynamically renders:
  - Entity name (id="share-entity-name")
  - Max value and FY (ids="share-max-value", "share-max-fy")
  - Min value and FY (ids="share-min-value", "share-min-fy")
- Updates the page’s <title> and <h1> with the live entity name.
- Supports an optional query param ?CIK=########## (10 digits) to load any other company via a CORS-friendly proxy without reloading the page.
- Provides a “Download data.json” button that saves the computed object in the required structure:
  {"entityName": "…", "max": {"val": …, "fy": "…"}, "min": {"val": …, "fy": "…"}}

Note: Browsers restrict setting the User-Agent header directly. The code attempts a descriptive header where possible and uses a proxy for alternate CIKs to satisfy the requirement to fetch alternate company data without reloading.

# Setup

- No build step required. It’s a single HTML file.

Files expected in the repository (the evaluation system may provide these separately):
- index.html (this app)
- data.json (computed output if saved)
- uid.txt (committed as provided)
- LICENSE (MIT License)

# Usage

- Open index.html in a modern browser.
- By default, it will request:
  https://data.sec.gov/api/xbrl/companyconcept/CIK0000732717/dei/EntityCommonStockSharesOutstanding.json
  and render AT&T’s figures.

- To view another company, append a 10-digit CIK in the URL:
  index.html?CIK=0001018724
  The app will fetch the alternate CIK using a proxy and update the entity name, title, and max/min fields in place.

- Click “Download data.json” to save the computed JSON (entityName, max, min) for the currently displayed company.

If you run into CORS limitations when fetching directly from the SEC endpoint, the app automatically falls back to a CORS-friendly proxy for alternate CIKs and on failures.

# Round

This is Round 1.