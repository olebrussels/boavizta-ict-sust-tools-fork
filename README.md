# 🗺️ Mapping of the ICT Sustainability Tools Landscape

This repository aim at providing to the global IT Sustainability community a vendor-neutral, fact-based, non-judgmental and exhaustive repository of the tools and services available to assess the environmental footprint of IT components.

It defines a data model that can be used to build an inventory of the tools.

> [!CAUTION]
> Today, any data contained in this repository is not validated nor reviewed. It is likely that the data contains completely false assumptions. It is here as sample data for the sole purpose of development and testing of the model. It should NOT be used for anything else.

> [!WARNING]
> The data model is still very drafty and is expected to evolve.

## Implementation

### General design

1. data stored in GIT to benefit from review / approval workflows (and potential automatic validation of format)
2. data update can be proposed by end users in 2 ways:
   1. A web frontend that allow to view the data and propose edition
   2. Pull requests directly against the data file (git)

We intend to use a [Datami](https://datami-docs.multi.coop/?locale=en) widget to display data and allow edition.

![Components of datami widget](doc/datami-components.excalidraw.png)

### Data format

- [x] We store data as a csv file.
- [ ] add specific descriptors as table-format to describe and validate constraints on the fields.
- [ ] implement frictionless-ci or other automatic validation regarding format as a github action [frictionless-ci | Frictionless Repository](https://repository.frictionlessdata.io/index.html)

> [!NOTE]
> Our preferred format would have been to have a structured (json) file, described by a json schema. However json edition is not well supported by Datami yet. (JSON data is displayed as json tree which is not very user friendly for non technical users). So for the time being we fall back to using a less structured CSV dataset.

### draft json schema

See the draft [schema](examples/json/ict-sustainailty-tools.draft.schema.json)

### draft dataset

See [draft dataset](ict-sustainability-tools.csv)


### Example Datami widgets

- A CSV file displayed without any customization [examples/csv/csv-widget-basic.html](examples/csv/csv-widget-basic.html)
- A CSV file displayed with some additional constraints on fields [examples/csv/csv-widget-with-constraints.html](examples/csv/csv-widget-with-constraints.html).
