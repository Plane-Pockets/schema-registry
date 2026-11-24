# Flight payload schema

`payload.xsd` defines the XML format for a flight-search payload. The document
must use a `<payload>` root and follow the element order defined by the schema.

## Fields

| Element | Type | Required |
| --- | --- | --- |
| `website` | string | Yes |
| `job_type` | string | Yes |
| `parameters/origin` | string | Yes |
| `parameters/destination` | string | Yes |
| `parameters/departure_date` | `xs:date` (e.g. `2026-12-15`) | Yes |
| `parameters/class` | string | Yes |
| `parameters/adults` | `xs:nonNegativeInteger` | Yes |
| `parameters/children` | `xs:nonNegativeInteger` | Yes |
| `parameters/infants` | `xs:nonNegativeInteger` | Yes |
| `proxy/server` | `xs:anyURI` | Only when `proxy` is present |
| `proxy/username` | string | Only when `proxy` is present |
| `proxy/password` | string | Only when `proxy` is present |

The entire `proxy` element is optional. When included, it must contain all
three fields in the order shown above.
## Example

```xml
<payload>
    <website>ixigo</website>
    <job_type>flight_search</job_type>
    <parameters>
        <origin>DEL</origin>
        <destination>BOM</destination>
        <departure_date>2026-12-15</departure_date>
        <class>e</class>
        <adults>1</adults>
        <children>0</children>
        <infants>0</infants>
    </parameters>
    <proxy>
        <server>http://proxy.example.com:8080</server>
        <username>your-username</username>
        <password>your-password</password>
    </proxy>
</payload>
```

## Result schema

`result.xsd` defines the XML format for a flight-search result. It uses a
`<result>` root, with the same fields and order as the JSON result. The
`<proxy>` element is optional. The `<results>` element contains zero or more
repeated `<flight>` elements.

Flight departure and arrival times use 24-hour `HH:mm` format. The price is a
string because it may include a currency symbol. The required `<error>` element
is nillable; represent a null error with
`<error xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:nil="true"/>`.

Validate XML documents with an XML Schema validator, using `payload.xsd` for
requests or `result.xsd` for results.
