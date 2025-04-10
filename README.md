# airbyte_databricks_cache

Databricks cache implementation for PyAirbyte

## Installation

```sh
pip install airbyte # pre-requisite
pip install airbyte-databricks-cache

```


## Usage

```py

import airbyte as ab
import airbyte_databricks_cache  # Must import to inject the module into airbyte.caches.databricks
from airbyte.caches.databricks import DatabricksCache # pylint: disable=E0401:import-error

# create airbyte source
source = ab.get_source(...)

# create DatabricksCache
cache_dbks = DatabricksCache(
    access_token = ab.get_secret("databricks_access_token"),
    server_hostname = ab.get_secret("databricks_server_hostname"),
    http_path= ab.get_secret("databricks_http_path"),
    catalog = ab.get_secret("databricks_catalog"),
    schema_name = ab.get_secret("databricks_target_schema"),
    staging_volume_w_location = ab.get_secret("databricks_staging_volume_w_location")
)

# read into cache
result = source.read(
    cache=cache_dbks,
    streams=['xxx'],
    force_full_refresh=False,
    write_strategy="append"
)

### OR
# write to destination
destination = ab.get_destination("destination-databricks", cache_dbks)


destination.write(source, ...)
```


## Build and deploy

Happens via github workflow.

```sh

# release
git fetch --tags origin
git describe --tags --abbrev=0
gh release create v0.1.8 --generate-notes
# gh release create v0.1.8 --target feat-workflow --generate-notes
git fetch --tags origin


## manual addons
# check long desc for pypi
twine check dist/*
```
