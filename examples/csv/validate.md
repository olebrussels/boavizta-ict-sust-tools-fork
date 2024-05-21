# Using frictionless validator to validate a CSV against the model

We can use Frictionlessdata tooling to validate a CSV against the model.

- [Using frictionless validator to validate a CSV against the model](#using-frictionless-validator-to-validate-a-csv-against-the-model)
  - [References](#references)
  - [Validate csv structure only](#validate-csv-structure-only)
  - [Generate a basic schema to describe the resource](#generate-a-basic-schema-to-describe-the-resource)
  - [Update the schema (named `filename.resource.yaml` with more constraints.](#update-the-schema-named-filenameresourceyaml-with-more-constraints)

## References

- [Validating Data | Frictionless Framework](https://framework.frictionlessdata.io/docs/guides/validating-data.html)
- [GitHub - frictionlessdata/frictionless-ci: Data management service that brings continuous data validation to tabular data in your repository via Github Action](https://github.com/frictionlessdata/frictionless-ci)

## Validate csv structure only

```sh
# structure only is checked
frictionless validate light-list.csv
```

## Generate a basic schema to describe the resource

```sh
# generate a basic schema
frictionless describe light-list.csv --yaml > light-list.resource.yaml
# validate data against ths schema (of course it passes)
frictionless validate light-list.resource.yaml
```

/!\ Name of the schema is important for latest steps to work, we need `<FILENAME>.resource.yaml` to validate `<FILENAME>.csv`.

## Update the schema (named `filename.resource.yaml` with more constraints.


Edit `light-list.resource.yaml` to add more rules.

For example add a description to the `tool_name` and force `assesment_type` to take values from an enum

```yaml
schema:
  fields:
    - name: tool_name
      title: tool_name
      description: The tool must have a well identified name and not being just an item
        in the menu of a bigger tool (e.g. not the “sustainability check” option in a
        development tool)
      type: string
    - name: organization
      type: string
    - name: website
      type: string
    - name: assessment_type
      description: Type of assessment the tool is designed for
      type: string
      constraints:
        enum:
        - Best practices
        - Evaluation
        - Measure
        - TBQ
```

Try a validation

```sh
# from the root of the repository
cd examples/csv

frictionless validate light-list.resource.yaml
```
