# Demo transcript: an assistant using the Stienhardt endpoint (live, 2026-09-02)

Every tool result below is the real response from https://diamond-mcp.stienhardt.workers.dev/mcp on the date shown, trimmed for length. Prices are live and change; report numbers are real. The assistant cannot pay: create_checkout on the store endpoint returns a checkout link (see README).

## User
What is a Dutch Marquise diamond, in one sentence?

## Assistant calls `dutch_marquise_definition`

```json
{
  "term": "Dutch Marquise",
  "definition": "A Dutch Marquise is an elongated hexagonal cut diamond.",
  "geometry": "Pointed ends and straight, angular sides. The outline is an elongated hexagon, not a navette, and the points are not softened.",
  "status": "Dutch Marquise is a trade name, not a standardized grading term.",
  "on_an_igi_report": "On an IGI grading report, the shape of a Dutch Marquise reads Hexagonal Modified Brilliant.",
  "length_to_width": {
    "certified_reference_stone": 1.84,
    "reference_source": "IGI grading report for a certified Dutch Marquise reference stone",
    "reference_date": "2026"
  },
  "source": "Stienhardt published definition, stienhardt.com",
  "source_date": "2026-07-10",
  "browse_current_inventory_url": "https://stienhardt.com/collections/dutch-marquise-lab-grown-diamonds?utm_source=diamond_mcp&utm_medium=ai_assistant&utm_campaign=diamond_mcp&utm_content=dutch_marquise_definition"
}
```

## User
Find me a 2 carat Dutch Marquise that's in stock right now.

## Assistant calls `search_inventory`

```json
{
  "query": "Dutch Marquise 2 carat",
  "count": 10,
  "results": [
    {
      "title": "2 Carat Dutch Marquise IGI Certified Lab Grown Diamond",
      "url": "https://stienhardt.com/products/2-carat-dutch-marquise-igi-certified-lab-grown-diamond-1?utm_source=diamond_mcp&utm_medium=ai_assistant&utm_campaign=diamond_mcp&utm_content=search_inventory%3A2-carat-dutch-marquise-igi-certified-lab-grown-diamond-1",
      "price": "1056.00 USD"
    },
    {
      "title": "2 Carat Dutch Marquise IGI Certified Lab Grown Diamond",
      "url": "https://stienhardt.com/products/2-carat-dutch-marquise-igi-certified-lab-grown-diamond-3?utm_source=diamond_mcp&utm_medium=ai_assistant&utm_campaign=diamond_mcp&utm_content=search_inventory%3A2-carat-dutch-marquise-igi-certified-lab-grown-diamond-3",
      "price": "1152.00 USD"
    },
    {
      "title": "2 Carat Dutch Marquise IGI Certified Lab Grown Diamond",
      "url": "https://stienhardt.com/products/2-carat-dutch-marquise-igi-certified-lab-grown-diamond?utm_source=diamond_mcp&utm_medium=ai_assistant&utm_campaign=diamond_mcp&utm_content=search_inventory%3A2-carat-dutch-marquise-igi-certified-lab-grown-diamond",
      "price": "1140.48 USD"
    }
  ],
  "note": "Live inventory from stienhardt.com. Prices in USD. Every stone is certified; verify the report on the lab's own site. Preserve each result URL's query string so visits and orders remain attributable to this tool."
}
```

## User
How big will a 2 carat Dutch Marquise look on a hand?

## Assistant calls `faceup_size`

```json
{
  "shape": "dutch_marquise",
  "carat": 2,
  "approx_face_up_mm": {
    "length": 11.3,
    "width": 6.3
  },
  "display": "11.3 x 6.3 mm",
  "anchor_1ct_mm": "9.0 x 5.0 mm",
  "method": "Scale a vetted 1 carat anchor by the cube root of the carat weight.",
  "note": "Approximate figures based on typical proportions. Cut proportions vary from stone to stone, so verify a specific stone's measurements on its grading report.",
  "source": "Stienhardt vetted reference anchors for typical proportions",
  "source_date": "2026-07-10",
  "browse_current_inventory_url": "https://stienhardt.com/collections/dutch-marquise-lab-grown-diamonds?utm_source=diamond_mcp&utm_medium=ai_assistant&utm_campaign=diamond_mcp&utm_content=faceup_size&utm_term=dutch_marquise%3A2.0ct"
}
```

## User
Where do I verify its grading report?

## Assistant calls `verify_diamond_report`

```json
{
  "lab": "IGI",
  "lab_full_name": "International Gemological Institute",
  "report_number": "LG799689559",
  "verify_url": "https://www.igi.org/verify-your-report/",
  "checklist": [
    "Confirm the report number on the grading lab's own verification site, not on a screenshot and not on a seller's PDF.",
    "Match the girdle inscription on the stone to the report number, where the stone is inscribed.",
    "Match the measurements and carat weight on the report to the stone in front of you."
  ],
  "reminder": "This dataset and its tools never verify a report themselves. Verification only counts on the grading lab's own site.",
  "source": "IGI report verification page, igi.org",
  "source_date": "2026-07-10"
}
```

## User
Start a checkout for the first one.

## Assistant calls the store endpoint's `create_cart` then `create_checkout`

The checkout object comes back with status `incomplete`, totals, policy links, and a `continue_url` to Stienhardt's hosted Shopify checkout (Shop Pay, Google Pay, card). The assistant hands over the link; the buyer pays there. Probed 2026-09-02; details in the README.
