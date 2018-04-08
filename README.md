# RESTful JSON Profile

This specification makes use of the conventions defined in [RFC 2119][].

> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL  NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119][].

## Overview

[RESTful JSON][] exists as a pattern and media type for adding links to JSON in a simple way. However, RESTful JSON alone is not enough to define the properties and links of that JSON. It requires an outside document to define this information, such as [ALPS][] or [OpenAPI][].

RESTful JSON Profile provides a minimal format for describing RESTful JSON properties and links. Like RESTful JSON, it is meant to provide just enough in order to get the full benefits of a hypermedia architecture.

It takes a few opinionated approaches. First, it removes type definitions. This can simplify API design and remove coupling between the client and server. Clients should marshall JSON into their own objects rather than rely on API types.

Second, it relies on one affordance per link. This is to simplify in design thinking and provide great granularity in expressing runtime affordances. If this is limiting to some designs, RESTful JSON Profile allows for defering the definition of a link to an outside link relation.

Third, it limits the definition of implementation details. The more details that can be pushed to runtime, the more flexible and evolvable and API can be. In defining a small set of the details, RESTful JSON Profile can describe just enough information that can be used to make HTTP requests.

### Inflences

This profile specification is influenced by some great technologies.

- [ALPS][] for its way for defining application semantics
- [HAL-FORMS][] for its way of defining link relation semantics
- [JSON Schema][] for its way of defining nested semantics and references
- [OpenAPI][] for its way of defining API descriptions

## Specification

### Profile Object (Base)

- `url` (string) - Link to profile. MAY be used as a way to version profiles.
- `properties` (Properties Object)
- `links` (Links Object)

### Properties Object

- *property-name* (enum)
    - (Base)
    - (Base) - For arrays of items
        - `items` (array)
            - (enum)
                - (Profile Object)
                - (Reference)
    - (Profile Object) - For describing nested profile objects
    - (Reference)

### Links Object

- *link-name* (enum)
    - (Base) - Defer definition to registered or extension link relation type
        - `rel` - Link relations as defined in [RFC 5988][]
    - (Base)
        - `implementation` (enum)
            - (Link Implementation Details)
            - (Reference)
        - `request_properties` - Note these properties are flat and do not allow for nested descriptions
            - *property-name* (Base)
                - `required`: false (boolean, default)
        - `responds_with` (enum) - Profile for the link response
            - (Profile Object)
            - (Reference)
    - (Reference)

### Reference

Used for including the definition to allow for reuse.

- `reference_url` - Should use URI fragment JSON Pointers ([RFC 6901][])

### Link Implementation Details

Link implementation details go beyond defining profile semantics and allow for specific implementation details.

- `method`: GET (string, default) - HTTP method as defined by [RFC 2616][] to be used for the link
- `encode_as`: application/json (string, default) - media type to use for encoding the request as defined by [RFC 6838][]

### Base

- `title` - Title for the context in which `title` is found
- `description` - Description for the context in which the `description` is found


[ALPS]: http://alps.io/
[HAL FORMS]: https://rwcbook.github.io/hal-forms/
[JSON Schema]: http://json-schema.org/
[RESTful JSON]: http://restfuljson.org/
[OpenAPI]: https://github.com/OAI/OpenAPI-Specification

[RFC 2119]: https://www.ietf.org/rfc/rfc2119.txt
[RFC 2616]: https://tools.ietf.org/html/rfc2616
[RFC 5988]: https://tools.ietf.org/html/rfc5988
[RFC 6838]: https://tools.ietf.org/html/rfc6838
[RFC 6901]: https://tools.ietf.org/html/rfc6901