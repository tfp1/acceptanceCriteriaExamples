### Problem

Migrating to a new rewards vendor requires us to change our tax settings are managed. Instead of tracking on a per-SKU basis, we manage them on a per-Catalog-Country basis where a `Catalog-Country` is a record for each catalog reduced to a single country. Some catalogs have many countries associated, but we want to have different tax settings by country since the tax rules vary by country.

This story is to fetch the tax settings data from the legacy app, transform it to match the new schema by unioning with the vendor's catalog data and then inserting into the new database.

### Prerequisites

Get list of companies in kw that need to have their tax settings migrated

Insert data into `kw.rewards.temp-tax-settings-company`

```workspace: subdomain,
legacy_rr_id: _id,
threshold: tax_reporting_minimum_amount,
countries: [array of alpha-2 codes],
isComplete?: FALSE
```

### Create a Daily task

* For each company in `kw.rewards.temp-tax-settings-company` with `isComplete? = false`, get vendor credentials from YEI
* Store credentials in `kw.rewards.Company`
* For each company in `kw.rewards.temp-tax-settings-company` with `isComplete? = false`, fetch `/catalogs` from vendor partner
*Skip all catalog-countries if the country is not in `kw.rewards.temp-tax-settings-company.countries`
* Union the data with `kw.rewards.temp-tax-settings-company.threshold` and store in `kw.rewards.Catalogs`
* Set `kw.rewards.temp-tax-settings-company.isComplete? = TRUE`

### Fetch the data from legacy DB
```
db.companies.aggregate(
{$match:{
    status: {$in:["active","prelaunch"]},
    "company_setting.tax_reporting": true
    }},
{$project:{
    _id:0,
    legacy_rr_id:"$_id", //We'll have to stringify this later with RegEx
    workspace:"$subdomain",
    threshold:"$company_setting.tax_reporting_minimum_amount",
    "isComplete?":"false", //Mongo doesn't let us set the boolean `false` here, so we have to de-stringify this with a find-replace later
    countries:[] //will be merged from a manually created CSV
    }}
).toArray()
```
