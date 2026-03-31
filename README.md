# teems-mappings

Set aggregation mappings for use with the [`teems`](https://github.com/teemsphere/teems-R) R package. Each mapping is a two-column CSV file that maps disaggregated (origin) set elements to their aggregated counterparts and can be passed directly to [`ems_data()`](https://teemsphere.github.io/data_load.html) by name.

## Repository structure

Mappings are organized by GTAP database version and data format:

```
teems-mappings/
├── GTAPv9/
│   ├── GTAPv6/        # Classic GTAP v6.2 format (TRAD_COMM, ENDW_COMM, REG, CGDS_COMM)
│   └── GTAPv7/        # Standard GTAP v7.0 format (COMM, ACTS, ENDW, REG)
├── GTAPv10/
│   ├── GTAPv6/
│   └── GTAPv7/
└── GTAPv11/
    ├── GTAPv6/
    └── GTAPv7/
```

## File format

Each mapping is a two-column CSV where the first column contains origin (disaggregated) set elements and the second column (`mapping`) contains destination (aggregated) elements:

```
REG,mapping
chn,asia
usa,oecd
deu,oecd
...
```

## Available mappings

### Region (`REG`)

Available for all database versions and both data formats.

| Name | Elements | Description |
|------|----------|-------------|
| `big3` | chn, usa, row | China, United States, Rest of World |
| `AR5` | asia, eit, lam, maf, oecd | IPCC Fifth Assessment Report regions |
| `WB7` | 7 regions | World Bank 7-region classification |
| `WB23` | 23 regions | World Bank 23-region classification |
| `R32` | 32 regions | IIASA [SSP mapping](https://tntcat.iiasa.ac.at/SspDb/dsd?Action=htmlpage&page=10) |
| `large` | ~15 regions | Broad regional groupings |
| `huge` | ~30 regions | Intermediate regional groupings |
| `full` | All | No aggregation |

The `AR5`, `WB7`, and `WB23` mappings are derived from the [countrycode](https://vincentarelbundock.github.io/countrycode/#/man/codelist) R package (`ar5`, `region`, and `region23` respectively).

### Sector (`TRAD_COMM`, `COMM`, `ACTS`)

| Name | Elements | Description |
|------|----------|-------------|
| `macro_sector` | crops, food, livestock, mnfcs, svces | Broad five-sector aggregation |
| `food` | ~15 sectors | Detailed food and agricultural breakdown |
| `energy` | ~20 sectors | Detailed energy sector breakdown |
| `manufacturing` | ~15 sectors | Detailed manufacturing breakdown |
| `services` | ~10 sectors | Detailed services breakdown |
| `medium` | ~30 sectors | Intermediate aggregation |
| `full` | All | No aggregation |

### Endowments (`ENDW_COMM`, `ENDW`)

| Name | Elements | Description |
|------|----------|-------------|
| `labor_agg` | capital, labor, land, natlres | Aggregated labor endowments |
| `labor_diff` | capital, land, natlres, sklab, unsklab | Skilled/unskilled labor differentiation |
| `full` | All | No aggregation |

## Usage

Internal mappings are referenced by name (without the `.csv` extension) in `ems_data()`. The set argument name must match the set as declared in the model file.

## External (custom) mappings

Custom mappings can be supplied as a path to a two-column CSV file, data frame, or data frame extension (e.g., tibble, data table) in the same format as the files in this repository. Column names are irrelevant -- first column will be read as the original element while the mapping is taken from the second column. The file path (or data frame object) is passed in place of the mapping name:

as a CSV file:

```r
data <- ems_data(
  dat_input = "path/to/gsdfdat.har",
  par_input = "path/to/gsdfpar.har",
  set_input = "path/to/gsdfset.har",
  REG       = "path/to/custom_REG_mapping.csv",
  COMM      = "macro_sector",
  ACTS      = "macro_sector",
  ENDW      = "labor_agg"
)
```

or where df_REG is a data frame:

```r
data <- ems_data(
  dat_input = "path/to/gsdfdat.har",
  par_input = "path/to/gsdfpar.har",
  set_input = "path/to/gsdfset.har",
  REG       = df_REG,
  COMM      = "macro_sector",
  ACTS      = "macro_sector",
  ENDW      = "labor_agg"
)
```

## Related repositories

- [`teems-R`](https://github.com/teemsphere/teems-R) — R package
- [`teems-models`](https://github.com/teemsphere/teems-models) — Vetted models
- [Documentation](https://teemsphere.github.io/) — Full manual
