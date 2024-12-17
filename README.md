# airbyte_databricks_cache

Databricks cache implementation for PyAirbyte

## Installation

```sh
pip install airbyte-databricks-cache

```


## Usage

```py

import airbyte as ab
import airbyte_databricks_cache # must import this in order to inject the modules into airbyte.caches.databricks
from airbyte.caches.databricks import DatabricksCache # pylint: disable=E0401:import-error
# from airbyte.caches.databricks import DatabricksCache

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

destination = ab.get_destination("destination-databricks", cache_dbks)


destination.write(source, ...)
```


## Build and deploy

Happens via github workflow.

```sh
# build via github actions

# release
git fetch --tags origin
gh release create v0.1.2 --target master --generate-notes
git fetch --tags origin


## manual addons
# check long desc for pypi
twine check dist/*
```
