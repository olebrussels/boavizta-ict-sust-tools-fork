# How-to configure the widget

- [How-to configure the widget](#how-to-configure-the-widget)
  - [1.Configure the table model](#1configure-the-table-model)
    - [Example with just 2 fields](#example-with-just-2-fields)
    - [Restricting allowed values](#restricting-allowed-values)
  - [2.Configure the display and edition of fields](#2configure-the-display-and-edition-of-fields)
    - [Basic Example](#basic-example)
    - [How to display a URL](#how-to-display-a-url)
    - [How to configure lists with multi valued vs mono valued inputs](#how-to-configure-lists-with-multi-valued-vs-mono-valued-inputs)
  - [3.Configure the cards view](#3configure-the-cards-view)
    - [Configure the cards](#configure-the-cards)
  - [4.Configure the widget](#4configure-the-widget)
  - [5.Tips to ease development](#5tips-to-ease-development)
  - [TODO](#todo)

Configuring the widget needs 4 steps:

1. configure the table model
2. configure the display/edition of fields in the table view
3. configure the cards view
4. configure the widget (html)

## 1.Configure the table model

Edit the ictst/model/tools.frictionless-table-schema.json file.

This files defines the name and type of the columns of the CSV. It uses the table schema syntax of [Table Schema | Data Package (v1)](https://specs.frictionlessdata.io/table-schema/).

We need to define one entry for each column of the CSV. 

**Mandatory** fields are:

- name
- title
- type

**Optional** fields (not sure that they are used by the widget) are:

- "constraints": "A constraint descriptor (for example to list allowed values)
- "description": "A description for the field"
- "example": "An example value for the field"
- "format": "A string specifying a format"

_⚠ Optional fields may not be used by the widget but may be used as documentation._

### Example with just 2 fields

```json
{
  "fields": [
    {
      "name": "tool_name",
      "title": "the name of the tool",
      "type": "string",
      "constraints": {"minLength":3},
      "description": "A long description about the field that is named tool_name",
      "example": "Boavizta API",
      "format": "A string specifying a format, but not used by the widget"
    },
    {
      "name": "organization",
      "title": "the organization behind the tool",
      "type": "string"
    }
  ]
}
```

### Restricting allowed values

Use the constraint field. 

It does **not** distinguish between mono or multiple values. This is done later at step 2 when configuring the widget.

```json
{
  "fields": [
    {
      "name": "status",
      "type": "string",
      "constraints": {
        "enum": [
          "started",
          "ready",
          "unknown"
        ]
      }
    },
    {
      "name": "languages",
      "type": "string",
      "constraints": {
        "enum": [
          "rust",
          "python",
          "docker",
          "other tek"
        ]
      }
    }
  ]
}
```




## 2.Configure the display and edition of fields

We need to configure how the widget displays and let the user edit the fields in a separate file.

Edit ictst/widget/tools.fields-custom-properties.json

>[!NOTE] The configuration of cards view (how content is displayed as cards insted of table is done separately.

### Basic Example

```json
{
  "fields": [
    {
      "name": "name",
      "sticky": true,
      "primaryKey": true,
      "maxLength": 50
    },
    {
      "name": "organization"
    }
  ]
}
    
```


### How to display a URL

Use `link` as the subtype.

"subtype": "link"

```json
{
  "name": "website",
  "subtype": "link"
}
```


### How to configure lists with multi valued vs mono valued inputs

Use the `tag`vs `tags`(plural) to let the widget know if the field accepts multiple values.

## 3.Configure the cards view

In addition to being displayed as a data table, Datami offers a _card_ view of each row.

A card view is displayed as:

- a _mini card_ (just a summary of the fields) that will will shown on a grid
- a _detail_ card (contains more fields and allows to edit the card content)

### Configure the cards

1. Activate the functionality: add a `cardsettings` entry to the widget configuration
2. Specify which field to display in the _mini_ and _detail_ view.

Example card view settings.

```json
"cardsview": {
    "activate": true,
    "default": false
  },
  "cardssettings": {
    "mini": {
      "tool_name": {
        "position": "title"
      },
      "organization": {
        "position": "resume"
      },
      "website": {
        "position": "links"
      }
    },
    "detail": {
      "tool_name": {
        "position": "title"
      },
      "organization": {
        "position": "resume"
      },
      "quick_description": {
        "position": "resume"
      },
      "website": {
        "position": "links"
      },
      "environmental_indicator": {
        "position": "tags"
      }
    }
  }
```

## 4.Configure the widget

## 5.Tips to ease development

During development phase it is tedious to use a separate configuration file or the widget. Because this configuration is declared as a github url in the html, it would involve constantly committing / pushing a change to test it.

💡 An alternative is to write the json configuration *inline* in the HTML file (see the example in [tools-widget-inline-dev.html](../ictst/widget/tools-widget-inline-dev.html)).

## TODO

Things that remain to be explained

- [x] Tag vs tags mention how to manage the separator for multiple values
- [ ] Configuration of the card view
- [ ] Explicit that multiline strings are not easily supported.
- [ ] Development flow (how to edit / test step by step)
- [ ] Example projet
- [ ] Hosting / deployment
- [ ] Github PAT configuration
- [ ] How to validate in data CI
