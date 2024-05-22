# 3. Use Datami to ease visualization or edition of data

Date: 2024-05-22

## Status

Status: Accepted on 2024-05-22  


## Context

The data is stored as CSV on GitHub but most users or contributors of the data may not be very familiar Github.

- We still want theses non-technical users to be able to visualize or edit (propose changes) on the data.
- We want to benefit from traceability and historisation of changes (Pull requests).
- We do want to maintain accounts or access rights on a specific system to allow to submit a update on data.
- We want to limit as much as possible the load on Boavizta benevolent members for the submission review process.

## Decision

We use Datami [https://datami-docs.multi.coop/?locale=en](https://datami-docs.multi.coop/?locale=en), an open source widget that allows users to visualize or contribute to data.

## Consequences

- Contributors or users do not need specific access rights to view/edit data.
- Need to customize configuration of widget to our needs (display and constraints on edition).
- Need to define a shared Github token to allow contribution from the widget.
- Need to choose where we will host the the public page that includes the widget.
