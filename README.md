# action-YAML-schema-validator

## Overview
This action can validate YAML files against provided JSON Schema. In case if YAML file is in full compliance
with provided JSON Schema action will exit gracefully, crash (unhandled exception) will occur otherwise.

The JSON Schema format used, should be in compliance with https://json-schema.org/. 

## Usage
To use action one can use code snippet below.
```yaml
jobs:
  my_cool_validation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Check my YAML file with my JSON schema
        id: validation
        uses: lyubick/action-YAML-schema-validator@v1
        with:
          json-schema-file: path/to/my/cool/schema.json
          yaml-file-dir: path/to/my/cool/yaml/file.yaml
          recursive: false
```
One should provide two parameters:
- `json-schema-file`, points to legit JSON Schema file
- `yaml-file-dir`, is a comma separated list that contains
  - Single YAML files
  - Directories that will be parsed for `.yaml` and `.yml` files
- `recursive`, True/False depending on if recursive scan for YAML files in directory required

## Results
### Success
In case of success action will end gracefully (exit code 0).

### Failure
In case of failure action will end with crash (unhandled exception), like on example below:
```text
File `invalid/yaml/file/here.yaml` failed validation with >>>`failure reason here`<<<

Traceback ...
```

## Examples
### JSON Schema
```json
{
  "title": "SampleJSONSchema",
  "description": "Just a sample and very simple JSON Schema",
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "field1": {
      "type": "string"
    }
  }
}
```
### YAML Valid file (compliant with Schema)
```yaml
field1: Value1
```

### YAML Invalid file (not compliant with Schema)
```yaml
field2: Value2
```
Validating this file will cause error (Exception) thus failing an action
```text
Failed validating 'additionalProperties' in schema:
    {'additionalProperties': False,
     'description': 'Just a sample and very simple JSON Schema',
     'properties': {'field1': {'type': 'string'}},
     'title': 'SampleJSONSchema',
     'type': 'object'}

On instance:
    {'field2': 'Value2'}
```
