# Missing Value Convention Metadata

- **UUID**: 78691ef5-ff17-4c55-98ca-a57f0e9d50bd
- **Name**: missing_value
- **Schema**: "https://raw.githubusercontent.com/zarr-experimental/missing_value/refs/tags/v0.1.0/schema.json"
- **Extension Maturity Classification**: Proposal
- **Owner**: @rouault

The document explains the "missing_value" convention, which is a Zarr convention
metadata.
Such value is used to represent undefined/invalid/missing values in an array.
This is distinct from the `Array.fill_value` property, which is used as the
return value for uninitialized chunks.
In a number of use cases, both `Array.fill_value` property and "missing_value"
can be set to the same value, but this is not required.

- Examples:
    - [Example for a uint8 data type](examples/missing_value_uint8.json)

## Configuration

The configuration in the Zarr convention metadata can be used in these parts of the Zarr hierarchy:

- [x] Array

| Field Name           | Type                      | Description                                  |
| -------------------- | ------------------------- | -------------------------------------------- |
| missing_value        | data_type dependent       | **REQUIRED**. Missing value sentinel         |

### Additional Field Information

#### missing_value

This field MUST be filled with the value of the missing value sentinel.
It uses the same type and encodings as the `Array.fill_value` property that
apply to the array. The missing value must be of the `Array.data_type`.

For core data types, they are described in the [permitted fill values](https://zarr-specs.readthedocs.io/en/latest/v3/data-types/index.html#permitted-fill-values) paragraph of the Zarr specification

For extended data type, they are described in the Fill value representation
paragraph of each [extended data type](https://github.com/zarr-developers/zarr-extensions/tree/main/data-types)
specification.

## Schema

See [schema.json](schema.json)

## Limitations

This approach, sometimes called a "sentinel value," is limited to data types for
which a sensible value can be definied (e.g. Nan for floating point types). It
is barely suitable for integers, especially ``int8`` or ``uint8`` with a small
range of values. Those use cases would benefit from a different approach.

## Decoded Representation of Missing Data

This spec is not perscriptive for how implementations should represent the decoded, in-memory representation of Arrays with missing data. This will vary by language and context. For example, Xarray uses NumPy Arrays with NaN values to represent missing data. Using Python Masked Arrays would be equally valid.

## Related conventions

There is prior art with the XArray "_FillValue" attribute which is defined
at https://github.com/zarr-developers/zarr-extensions/pull/24
