# 2. Use CSV as data format

Date: 2024-05-22

## Status

Status: Accepted on 2024-05-22  


## Context

We want the data set to be easily editable, with minimal tooling or account creation.

## Decision

We use CSV as the data format.

## Consequences

- The model/data dictionary of this CSV file needs to be carefully described and this documentation has to be public.
- CSV does not provides direct way to reference a schema, we need to use a external mechanism to ensure its consistency.
