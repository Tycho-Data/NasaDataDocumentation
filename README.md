# Tycho Data

**Space and sun data, cleaned for builders.**

Public space and Earth-observation data is free, but it is scattered across APIs with different units, fill values, paging rules and uptime. Tycho Data does the cleanup once and serves it as one stable, documented, versioned REST API, so you can build on the data instead of wrangling it.

## Where the data comes from

| Source | What we use it for |
| --- | --- |
| [NASA/JPL Small-Body Database Close-Approach Data API](https://ssd-api.jpl.nasa.gov/doc/cad.html) | Near-Earth object close approaches |
| [NASA POWER](https://power.larc.nasa.gov/) (daily and climatology endpoints) | Solar irradiance, temperature and long-term climate averages |

Tycho Data is not endorsed by or affiliated with NASA. Derived fields are convenience calculations and not an official impact risk assessment.

## What you get

| Endpoint | Purpose |
| --- | --- |
| `GET /v1/neo/approaches` | Close approaches with size and proximity classes, filters and cursor paging |
| `GET /v1/solar/daily` | Daily irradiance and temperature for a location, with a quality flag per day |
| `GET /v1/solar/monthly` | Monthly mean irradiance, missing days, rough specific yield and difference from climatology |
| `POST /v1/alerts` | Signed webhook alerts for close approaches |
| `GET /v1/alerts` | List your alert subscriptions |
| `DELETE /v1/alerts/{id}` | Remove an alert subscription |
| `GET /v1/health` | Service status |

Interactive documentation with example calls for every endpoint is available at `/v1/docs`.

## What "cleaned" means

- **Consistent units.** Distances in AU, kilometres and lunar distances, side by side.
- **One time format.** ISO 8601 in UTC, everywhere.
- **No duplicates.** Close approaches are de-duplicated and designations are tidied.
- **Honest gaps.** Upstream fill values such as `-999` become `null`, and days without data are flagged `missing` instead of silently skewing your averages.
- **Predictable locations.** Solar coordinates snap to the 0.5 degree grid cell, and the cell is returned with the data.
- **Derived fields done the same way every time.** Size and proximity classes use documented formulas, so results are comparable across queries.
- **Easy paging and fast responses.** Opaque cursors and caching come built in.

## Why use Tycho Data instead of doing it yourself

- **Skip the plumbing.** No fill-value handling, unit conversion, de-duplication, grid logic or retry code to write and maintain.
- **One integration.** One API, one schema, one auth model, instead of a separate client per upstream.
- **Stable contract.** Versioned endpoints, so an upstream format change does not break your app.
- **Insulation from upstream hiccups.** Caching smooths over slow or unavailable source APIs.
- **Ready-made features.** Monthly yield estimates, climatology comparison, quality flags and signed webhook alerts are things you would otherwise build yourself.
- **Your time goes to your product.** Spend it on what makes your product different.

### Example

```
GET /v1/neo/approaches?date_from=2026-11-01&date_to=2026-11-30&max_distance_au=0.05&limit=1
```

```json
{
  "count": 1,
  "data": [
    {
      "designation": "2024 AB",
      "approach_time": "2026-11-05T12:00Z",
      "distance_au": 0.01,
      "distance_km": 1495978.7,
      "distance_lunar": 3.892,
      "velocity_kms": 7.5,
      "closer_than_moon": false,
      "proximity_class": "within_10_ld",
      "size_class": "medium"
    }
  ],
  "source": "NASA/JPL SBDB Close-Approach Data API"
}
```

Sample values shown for illustration.

## What is coming

| Planned | What it means for you |
| --- | --- |
| **Recurring solar reports** | Monthly summaries delivered to you, ready to forward to customers or drop into a dashboard |
| **More alert types and delivery options** | Beyond close approaches, so you can react to more events automatically |
| **Additional cleaned datasets** | More public space and Earth-observation sources behind the same API |
| **Open-source data-cleaning core** | The cleaning logic released under Apache License 2.0 in a separate public repository |
| **Free science tier** | Part of our net revenue goes into free access and open-source work for students, researchers and educators |

Planned items are goals, not commitments, and may change.

## Ideas for expansion

We are interested in hearing what you would build. Likely directions include energy and solar-yield planning tools, education and planetarium content, research workflows and monitoring dashboards. Requests for datasets or features are welcome.

## Contact and support

Open an issue in this repository for questions, dataset requests or feature ideas.

## License

Copyright 2026 Tycho Data. All rights reserved.

## The name

Tycho Brahe kept the most precise naked-eye observations of his age. Kepler derived the laws of planetary motion from them. Good data makes discovery possible.
