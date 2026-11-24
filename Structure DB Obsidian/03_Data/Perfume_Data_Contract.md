# Perfume use case and handoff (synthetic-first)
Screenshots inform the desired shape only; do not scrape/reproduce the source. Fragrantica live access blocked in Source_Registry. Build generic perfume fixtures with fictional content.
| Entity / section | Fields |
|---|---|
| Brand list | source_brand_id, brand_name, brand_url, logo_ref, displayed_product_count |
| Brand detail | brand_name, declared country, catalogue links, optional attributes |
| Perfume list | source_perfume_id, source_brand_id, name, URL, image_ref, displayed year if present |
| Perfume detail hero | name, brand, target_audience, concentration, release_year, perfumer, declared family, image_ref |
| Accords | array of label, rank, raw displayed value if present; visual bar width only as measured visual relative value, never assumed percent |
| Ratings | mean, scale, vote_count, review_count, sentiment buckets with raw displayed counts/approximate flag |
| Wear | array season/day/night, displayed count and unit; not objective recommendation |
| Pyramid | arrays top/middle/base, note_name, note_ref, image_ref; declared notes not chemical formula |
| Optional | longevity, sillage, comparisons, description facts if licensed, no bulk review bodies |
A three-stage recipe chain (brand index -> permitted brand catalogue -> permitted perfume detail) is a conceptual use case tested on synthetic fixtures, not an executable plan for prohibited sites. Support alternate sources with field mapping. Counts shown abbreviated (e.g. k) are approximate; preserve display text and avoid fabricating exact value. Missing fields null + reason. Source IDs separate from parent canonical IDs. No confusion between ratings and similarity. Images refs only until rights for download/store/reuse approved.
Parent handoff package: manifest (contractVersion, source grants, job IDs, timestamp, hashes, counts), brands.json, perfumes.json, perfume_notes.json, perfume_accords.json, observations.json, evidence.json, validation.json and quarantine.json; CSV optional. Parent validates its current accepted schema/ADR, resolves aliases/dedup, stages import, dry-run, human approval and rollback in separate parent PR. No direct parent DB write, no production credentials, no automatic import, no invented facts. Parent contract mapping still requires live schema review before P7.
