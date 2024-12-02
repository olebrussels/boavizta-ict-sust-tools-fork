# Technical documentation

- [Technical documentation](#technical-documentation)
  - [Repository organization](#repository-organization)
  - [General design](#general-design)
    - [Architecture decisions records / ADRs](#architecture-decisions-records--adrs)
    - [Usage flow](#usage-flow)
    - [Components](#components)
  - [How to configure the widget](#how-to-configure-the-widget)
    - [Data format](#data-format)
    - [data validation in CI](#data-validation-in-ci)
    - [Other examples of Datami widgets configration](#other-examples-of-datami-widgets-configration)
  - [Local development](#local-development)
    - [1. Install the mini server for local development](#1-install-the-mini-server-for-local-development)
      - [Run local server](#run-local-server)
    - [2. Configure the widget to use the local files](#2-configure-the-widget-to-use-the-local-files)


## Repository organization

- index.html : source of the visualization page (published at [https://boavizta.github.io/ict-sustainability-tools/](https://boavizta.github.io/ict-sustainability-tools/))
- doc/ : documentation
  - architecture decision records: [doc/adr/](doc/adr/)
- ictst
  - ictst/data
    - Raw data file (CSV): [ictst/data/tools.csv](ictst/data/tools.csv)
    - Data validation model: [ictst/data/tools.resources.yaml](ictst/data/tools.resources.yaml): Used in command line or via github actions.
  - ictst/model
    - Data model that describes the format of the CSV file (used to configure the widget): [ictst/model/tools.frictionless-table-schema.json](ictst/model/tools.frictionless-table-schema.json)
  - ictst/widget:
    - sample html page (widget): [ictst/widget/tools-widget.html](ictst/widget/tools-widget.html)
    - additional fields configuration for the widget [ictst/widget/tools.fields-custom-properties.json](ictst/widget/tools.fields-custom-properties.json)
- .github/workflows : github action to automate the data validation and publication of the page
- doc/old-examples/: other examples of using or configuring the Datami widget

## General design

We use a [Datami](https://datami-docs.multi.coop/?locale=en) widget to display data and allow edition of a file stored in Github. Even if end user is not familiar with Git.

1. data is stored in GIT to benefit from historisation, review and approval workflows (and potential automatic validation of format)
2. data update can be proposed by end users in 2 ways:
   1. A web frontend that allow to view the data and propose edition => This is the **preferred** solution using Datami widget
   2. Pull requests directly against the data file (git) => **Less preferred** (mainly for maintainers of people familiar with Git)

### Architecture decisions records / ADRs

- [All ADRs](adr/flowChart.md)

### Usage flow

![usage flow](usage-flow.excalidraw.png)

### Components

![Components of Datami widget](datami-components.excalidraw.png)

> [!WARNING]
> The data validation and the data edition (widget) are configured using different set of files or data models.
> These data model use different syntax but have to be kept in sync manually !

## How to configure the widget

[How to configure the widget](doc/how-to-configure-widget.md)

### Data format

- [x] We store data as a csv file.
- [x] add specific descriptors as table-format to describe and validate constraints on the fields.
- [x] implement frictionless-ci or other automatic validation regarding format as a github action [frictionless-ci | Frictionless Repository](https://repository.frictionlessdata.io/index.html)

> [!NOTE]
> Our preferred format would have been to have a structured (json) file, described by a json schema. However json edition is not well supported by Datami yet. (JSON data is displayed as json tree which is not very user friendly for non technical users). So for the time being we fall back to using a less structured CSV dataset.

### data validation in CI

Data _format_ (not content) is automatically validated in CI using frictionless-ci in a github action triggered on PR's.

See [frictionless-ci | Frictionless Repository](https://repository.frictionlessdata.io/index.html)

```sh
# Local data validation
# See https://framework.frictionlessdata.io/docs/guides/validating-data.html

# Install frictionless cli
pip install frictionless
# 
cd ictst/data
# validate just csv _global_ structure
frictionless validate tools.csv
# validate that format of fields matches description
frictionless validate tools.resources.yaml
```

### Other examples of Datami widgets configration

- A CSV file displayed without any customization [doc/old-examples/csv/csv-widget-basic.html](doc/old-examples/csv/csv-widget-basic.html)
- A CSV file displayed with some additional constraints on fields [doc/old-examples/csv/csv-widget-with-constraints.html](doc/old-examples/csv/csv-widget-with-constraints.html).
- Other examples with more complex validation rules:[GitHub - demeringo/datami-tests: Testing datami widget to edit and validate csv files](https://github.com/demeringo/datami-tests/)

---

## Local development

To ease development and configuration of the widget, we can work with a local data set and local configuration files.

1. Run a local server to expose the *data* and *configuration* files locally (see below).
2. Modify `index.html` to use the local files (configure the `localdev` properties to `true`).

### 1. Install the mini server for local development

A mini server is written in the `server.py` to serve this folder's files.

To install the mini-server :

```sh
pip install --upgrade pip
python3 -m pip install --user virtualenv
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

or

```sh
sh setup.sh
source venv/bin/activate
```

---

#### Run local server

To run the server on `http://localhost:8800`:

```sh
python server.py
```

or

```sh
sh run_server.sh
```

Files will be locally served on :

- `http://localhost:8800/content/<path:folder_path>/<string:filename>`
- `http://localhost:8800/statics/<path:folder_path>/<string:filename>`

### 2. Configure the widget to use the local files

Modify `index.html` to use the local files (configure the `localdev` properties to `true`)

```json
{
        "title": "Tools Landscape",
        "activate": true,
        "localdev": true,
        "gitfilelocal": "http://localhost:8800/statics/ictst/data/tools.csv",
        "gitfile": "https://github.com/Boavizta/ict-sustainability-tools/blob/main/ictst/data/tools.csv",

[....]

 "schema": {
            "localdev": true,
            "filelocal": "http://localhost:8800/statics/ictst/model/tools.frictionless-table-schema.json",
            "file": "https://github.com/Boavizta/ict-sustainability-tools/blob/main/ictst/model/tools.frictionless-table-schema.json"
          },
          "fields-custom-properties": {
            "localdev": true,
            "filelocal": "http://localhost:8800/statics/ictst/widget/tools.fields-custom-properties.json",
            "file": "https://github.com/Boavizta/ict-sustainability-tools/blob/main/ictst/widget/tools.fields-custom-properties.json"
          },
```

💡 You can now open the local `index.html` in a web browser and update data or configurations. Reload the page and changes are immediately visible.