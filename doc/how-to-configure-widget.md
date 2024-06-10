# How-to configure the widget

- [How-to configure the widget](#how-to-configure-the-widget)
  - [1.Configure the table model](#1configure-the-table-model)
    - [Example with just 2 fields](#example-with-just-2-fields)
    - [Restricting allowed values](#restricting-allowed-values)
  - [2.Configure the display of fields](#2configure-the-display-of-fields)
    - [Multi valued vs mono valued inputs](#multi-valued-vs-mono-valued-inputs)
    - [Example](#example)
  - [3.Configure the widget](#3configure-the-widget)
  - [TODO](#todo)

Configuring the widget needs 3 steps:

1. configure the table model
2. configure the display of fields
3. configure the widget (html)

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




## 2.Configure the display of fields

We need to configure how the widget displays and let the user edit the fields in a separate file.

Edit ictst/widget/tools.fields-custom-properties.json

### Multi valued vs mono valued inputs

Use the `tag`vs `tags`(plural) to let the widget know if the field accepts multiple values.

### Example

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

## 3.Configure the widget

## TODO

Tag vs tags

Development flow (step by step)

Example projet

Hosting constraints

