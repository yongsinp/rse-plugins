---
name: download-script-dev
description: Use this skill when users ask to "develop a download script", "debug data download", "fix download error", "create data pipeline template", "download template", "GAIA data pipeline", "download from S3", "access Zarr store", "cloud data access", or mention sources like CONUS404, HRRR, WRF, PRISM, Stage IV, USGS, ORNL, DEM, Synoptic, or IRIS. Generates CONFIG-at-top Python download scripts, validates source-specific configuration (auth, endpoint/path, date range, variables, AOI/CRS, and output format), and diagnoses common failures (403/auth, timeout/retry, CRS mismatch, missing binaries, and partial downloads).
version: 2026-05-31
---

# Download Script Development Skill

## Overview

Assist in developing, refining, and debugging data download scripts for GAIA hydroclimatological data sources. This skill provides templates, configuration schemas, and troubleshooting guidance for building reproducible data pipelines across 10+ environmental data sources.

## Use When

- The user asks for a new download script or pipeline template for a GAIA source.
- The user is debugging data download failures (auth 403, timeout, CRS mismatch, missing binaries, partial downloads).
- The user needs help choosing access pattern/library for CONUS404, HRRR, WRF, PRISM, Stage IV, USGS, ORNL, DEM, Synoptic, or IRIS.
- The user needs source-specific config validation before running a large download.

## Requirements

- **Python 3.9+** with `xarray`, `geopandas`, `rioxarray`
- **Source-specific libraries:** `herbie-data` (HRRR), `pyPRISMClimate` (PRISM), `obspy` (IRIS), `boto3` (WRF/S3), `elevation` (DEM), `s3fs` (CONUS404)
- **System dependencies:** `wgrib2` for HRRR (install via conda-forge, not pip)

## Script Structure Pattern

All download scripts follow a CONFIG-at-top pattern separating parameters from logic:

```python
import xarray as xr
import geopandas as gpd
from concurrent.futures import ThreadPoolExecutor

# ============================================================
# Configuration — modify these parameters before running
# ============================================================
CONFIG = {
    "source": "SOURCE_NAME",
    "date_range": ("2024-01-01", "2024-01-31"),
    "variables": ["var1", "var2"],
    "aoi_path": "../data/GIS/boundary.json",
    "output_path": "../data/output.zarr",
    "output_format": "zarr",
    "max_workers": 8,
}
# ============================================================
# Download logic — generally no need to modify below this line
# ============================================================

def validate_config(cfg):
    required = ["source", "date_range", "variables", "output_path", "output_format"]
    missing = [k for k in required if k not in cfg or cfg[k] in (None, "", [])]
    if missing:
        raise ValueError(f"Missing config keys: {missing}")


def validate_aoi(aoi):
    if aoi.empty:
        raise ValueError("AOI is empty")
    if aoi.crs is None:
        raise ValueError("AOI CRS is missing")


def main():
    # 0. Validate configuration
    validate_config(CONFIG)

    # 1. Load + validate AOI
    aoi = gpd.read_file(CONFIG["aoi_path"])
    validate_aoi(aoi)

    # 2. Download (parallel) with retry + failure capture
    #    - collect failed items for a second pass
    #    - fail hard only if failures remain after retries

    # 3. Validate download completeness before combining
    #    - check expected_count vs downloaded_count
    #    - assert required variables are present

    # 4. Combine datasets and validate dimensions/time coverage
    #    - assert non-empty time axis

    # 5. Reproject AOI if needed, spatial subset, validate bounds
    #    - assert subset intersects AOI and is not empty

    # 6. Derive variables (if requested) and validate units/ranges

    # 7. Save output and verify artifact exists + can be reopened

    # 8. Print QC summary (counts, date range, spatial bounds, failed items)

if __name__ == "__main__":
    main()
```

## Four Data Access Patterns

### 1. Direct HTTP Download (PRISM, Stage IV, DEM)

Key differentiator: fixed file URLs with stateless downloads.
Usage: `session.get(url, timeout=60)` in a parallel loop with retry/backoff.

### 2. REST API Query (USGS, Synoptic)

Key differentiator: parameterized queries returning JSON/RDB payloads.
Usage: build params from `CONFIG`, call endpoint, then parse JSON or RDB.

### 3. Cloud Object Storage / S3 (CONUS404, HRRR, WRF-CMIP6)

Key differentiator: object-store access with lazy reads and partial loading.
Usage: open with `s3fs`/`boto3` + `xarray.open_zarr()` using anon/unsigned auth for public buckets.

### 4. Specialized Libraries (Herbie for HRRR, pyPRISMClimate, obspy for IRIS)

Key differentiator: source-specific clients that encapsulate URL/auth/parsing details.
Usage: call the library API with CONFIG parameters, then normalize output into the standard pipeline.

## Spatial Subsetting Methods

Choose based on grid type:

| Grid Type | Method | When to Use |
|-----------|--------|-------------|
| Regular (lat/lon) | `ds.rio.clip(aoi.geometry)` | PRISM, Stage IV, DEM |
| Curvilinear (model) | `regionmask` | CONUS404, WRF-CMIP6 |
| Irregular (points) | `shapely.contains()` | USGS station data |

Ensure the AOI CRS matches the data CRS before subsetting. Model grids often use Lambert Conformal Conic — reproject the AOI with `aoi.to_crs(ds.rio.crs)`.

## Parallel Download Pattern

```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def download_item(item):
    """Download a single item. Return path or dataset."""
    # ... download logic
    return result

with ThreadPoolExecutor(max_workers=CONFIG["max_workers"]) as executor:
    futures = {executor.submit(download_item, item): item for item in items}
    for i, future in enumerate(as_completed(futures), 1):
        result = future.result()
        print(f"Downloaded {i}/{len(items)}")
```

Worker count guidance: 4-8 for HTTP downloads, 8-16 for S3 reads, 2-4 for API endpoints with rate limits.

## Output Formats

| Format | When to Use | Trade-offs |
|--------|-------------|------------|
| **Zarr** (preferred) | Large gridded datasets, cloud workflows | Fast parallel I/O, chunked, no single-file limit |
| **NetCDF** | Sharing with traditional tools, small datasets | Widely supported, single-file, 2 GB limit (classic) |
| **CSV** | Tabular station data (USGS) | Human-readable, no spatial metadata |

## Common Issues and Debugging

### wgrib2 Not Found (HRRR)

`wgrib2` is a C binary, not pip-installable. Install via conda-forge: `conda install -c conda-forge wgrib2` or `pixi add wgrib2`. Verify with `shutil.which("wgrib2")`. Add a runtime guard in scripts:

```python
import shutil
assert shutil.which("wgrib2"), "wgrib2 not found. Install: conda install -c conda-forge wgrib2"
```

### S3 Authentication Errors (CONUS404, WRF)

Public buckets require anonymous access. For CONUS404: set `anon=True` in `s3fs.S3FileSystem()`. For WRF: use `botocore.UNSIGNED` config in boto3.

```python
# CONUS404
fs = s3fs.S3FileSystem(anon=True, client_kwargs={"endpoint_url": "https://usgs.osn.mghpcc.org"})

# WRF-CMIP6
from botocore import UNSIGNED
from botocore.config import Config
s3 = boto3.client("s3", config=Config(signature_version=UNSIGNED))
```

### CRS Mismatch During Spatial Subsetting

If `rio.clip()` raises a CRS error, reproject the AOI to match the dataset:

```python
aoi = aoi.to_crs(ds.rio.crs)  # reproject AOI to dataset CRS
ds_clipped = ds.rio.clip(aoi.geometry)
```

For datasets without CRS metadata, set it explicitly: `ds.rio.write_crs("EPSG:4326", inplace=True)`.

### Memory Issues with Large Datasets

Use chunked loading to avoid loading entire datasets into RAM:

```python
ds = xr.open_dataset(path, chunks={"time": 100})  # lazy loading
print(f"Dataset size: {ds.nbytes / 1e9:.1f} GB")   # check before computing
```

Process in temporal batches rather than loading the full dataset. Use `.compute()` only on subsets.

### Network Timeouts and Retries

Wrap downloads in retry logic with exponential backoff:

```python
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

session = requests.Session()
session.mount("https://", HTTPAdapter(max_retries=Retry(total=3, backoff_factor=2)))
response = session.get(url, timeout=60)
```

For S3, boto3 has built-in retry configuration via `botocore.config.Config(retries={"max_attempts": 3})`.

### USGS RDB Format Parsing

USGS returns tab-separated RDB format with comment headers (`#`) and a data-type row below the column headers. Parse by skipping both:

```python
lines = [l for l in response.text.splitlines() if not l.startswith("#")]
df = pd.read_csv(io.StringIO("\n".join([lines[0]] + lines[2:])), sep="\t")
```

Note: USGS returns data in **local time zones**. Convert to UTC using station timezone metadata.

## Additional Resources

### Reference Files

For detailed data source documentation, code templates, and configuration schemas, consult:

- **`references/sources/`** — Per-source documentation files (e.g., `sources/hrrr.md`, `sources/conus404.md`): endpoints, response formats, authentication setup, and example API calls. Load only the source relevant to the current task.
- **`references/DOWNLOAD_PATTERNS.md`** — Complete code templates for each access pattern with full pipeline examples for HRRR, CONUS404, and USGS
- **`references/CONFIGURATION.md`** — Per-source parameter tables with types, defaults, and validation rules; size estimation formulas
