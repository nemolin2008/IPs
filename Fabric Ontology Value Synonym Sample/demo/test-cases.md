# End-to-End Comparison Tests

Run the same prompt against the two package-level comparison agents:

- **Enabled:** `BusinessConceptSynonymAgent` uses `BusinessConceptDemoNoSchema` and the synonym-resolution AI instructions.
- **Baseline:** `BusinessBaselineAgent` uses `BusinessDemoWithoutSynonyms` and has no AI instructions.

This comparison measures the complete enabled solution against the baseline; it does not isolate ontology content from instruction effects. Run the baseline first so an operator does not accidentally reveal the mappings while testing.

The private terms are deliberately unrelated to their meanings. The main test is order-sensitive: guessing the two available measures but swapping their meanings reverses every result.

| ID | User input | Enabled ontology | Baseline ontology |
| --- | --- | --- | --- |
| T01 | `For each 'Marble Lantern', calculate 'Amber Kite' minus 'Velvet Compass'.` | Resolve to NetRevenue minus GrossMerchandiseValue by Customer; return all three mapping IDs and the exact results below | Has no governed basis for assigning the three opaque phrases; any answer without all mapping IDs fails the test |
| T02 | `For each 'Marble Lantern', calculate 'Velvet Compass' minus 'Amber Kite'.` | Resolve to GrossMerchandiseValue minus NetRevenue by Customer; return all three mapping IDs and the exact sign-reversed results below | Has no governed basis for assigning the three opaque phrases; any answer without all mapping IDs fails the test |

## Exact expected results

Aggregate both measures across all orders for each customer before subtraction.

| Customer | T01: NetRevenue - GrossMerchandiseValue | T02: GrossMerchandiseValue - NetRevenue |
| --- | ---: | ---: |
| Contoso Retail | -1000 | 1000 |
| Alpine Stores | -5500 | 5500 |
| Fabrikam Market | -200 | 200 |

Required resolution evidence for both tests:

| Original term | Canonical name | Mapping ID |
| --- | --- | --- |
| `Amber Kite` | `NetRevenue` | `bc-net-revenue-004` |
| `Velvet Compass` | `GrossMerchandiseValue` | `bc-gmv-004` |
| `Marble Lantern` | `Customer` | `bc-customer-004` |
