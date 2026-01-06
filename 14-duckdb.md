# DuckDB

## Current .duckdbrc

```
-- Extensions
INSTALL httpfs;
LOAD httpfs;
INSTALL aws;
LOAD aws;
INSTALL delta;
LOAD delta;

CREATE SECRET (TYPE S3, PROVIDER CREDENTIAL_CHAIN);
```