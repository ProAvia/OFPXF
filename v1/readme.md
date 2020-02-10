# Open Fuel Price Exchange Format

Version `v1`

## File extension

If stored within a file, the format uses the file extension `.ofpx `.

## Encoding

The file content shall be encoded using `UTF-8` only.

## Date and time

All date and time values within the document have to be in and are treated as `UTC` unless otherwise specified.

The `dateTime` format needs to match ISO 8601 as [described in the XSD reference](https://www.w3.org/TR/xmlschema-2/#dateTime).
The `time` format needs to match ISO 8601 as [described in the XSD reference](https://www.w3.org/TR/xmlschema-2/#time)

## Validation

The format is based on a valid XML document and can be validated against the format's [XSD schema](schema.xsd).
Please see the [example document](example.ofpx) as well.

## Inheritance

All listed properties which are provided for a single price will overwrite the globally set value for the respective item.

- Currency
- Quantity Unit
- Validity
- Pre-order

## Root element

Required. The root element is named `data` and requires the following attributes.

- The name of the `publisher` (string).
- The `quantityUnit` of the prices within the document (see list of [quantity unit types](enums.md#quantity-unit-type)).
- The `currency` of the prices within the document (see list of [currency types](enums.md#currency-type)).
- The date and time the file has been `issued` (dateTime).

## Optional elements below root

### Document validity element

Optional. As a child of the `data` element, one or zero element is allowed which describes validity period of the document. It's called `valid` and requires the attributes `from` and `until` (each of the type dateTime).

### Pre-order element

Optional. As a child of the `data` element, one or zero element is allowed which specifies whether ordering fuel via any of the data records within this document requires a `preOrder`. Therefore, the `required` (boolean) attribute is required. Additionally, the optional `leadTime` (time) attribute describes the time needed between an order and the actual ability to provide fuel.

## Items collection

Required. As a child of the `data` element, one element with the name `items` is required. It contains a [collection of](#item-element) `item` elements.

## Item element

As a child of the `items` element, any number of elements with the name `item` are allowed. Several child elements describe the actual price and its conditions.

### Price element

Required. The `price` element is required on each item, as well as its `price` (decimal) attribute. Optionally, this element can hold a `quantityUnit` and `currency` as well.

### Minimum uplift element

Optional. If the specified `price` is only available if the uplift is greater than a certain amount, it can be specified in the `threshold` attribute. Optionally, this element can hold a `quantityUnit` ([quantity unit types](enums.md#quantity-unit-type)) and `currency` ([currency types](enums.md#currency-type)) as well.

### Pre-order element

Optional. The `preOrder` element is equal to the [pre-order element of the document](#pre-order-element).

### Validity

Optional. The `valid` element is equal to the [document validity element](#document-validity-element).

### Operating hours

Optional. The `operatingHours` element defines the timespan per day in which fuel can be provided via the attributes `from` (time) and `until` (time) and `timeType` ([time types](enums.md#time-type)).

### Flags

Optional. The `flags` element contains blank-separated list of flags for its item. See [list of supported flags](enums.md#flags).

### Meta

Optional. The `meta` element holds several optional attributes which describes the price.

- `intoPlaneAgent` (string)
- `supplier` (string)
- `phone` (string)
- `paymentType` ([payment types](enums.md#payment-type))

### Remark

Optional. The `remark` (string) element can contain free text.

### Fees

Optional. The `fees` element can contain either pre-defined or custom fees which apply to this price item. Every element below this element requires a `price` (decimal) and can optionally have a `currency` ([currency types](enums.md#currency-type)) attribute.

#### Per uplift fee

Optional. The `perUplift` element defines the fee applicable per uplift.

#### Cancellation fee

Optional. The `cancellation` element defines the fee applicable when cancelling an order.

#### Low volume fee

Optional. The `lowVolume` element defines the fee applicable when ordering fuel below a certain volume. The required attributes `theshold` (decimal) and `thresholdUnit` ([threshold unit types](enums.md#quantity-unit-type)) describe the limit.

#### Overtime fee

Optional. The `overtime` element defines the fee applicable when receiving fuel outside the regular opening hours. The required attributes `from` (time), `until` (time) and `timeType` ([time type](enums.md#time-type)) describe the boundaries.

#### Custom fee

Optional. The `custom` element can occur zero to unlimited times and defines custom fees. Therefor, the `description` (string) attribute is required.