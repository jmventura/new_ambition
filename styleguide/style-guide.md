# BNL RESTful API guidelines

## 1. Introduction <a id="100"></a>

Modern software architectures center around decoupled microservices that provide functionality via RESTful APIs with a JSON payload. APIs most purely express what systems do, and are therefore highly valuable business assets. Designing high-quality, long-lasting APIs has become even more critical as modern companies strategy aims to develop lots of public APIs for external business partners to use via third-party applications, leveraging new busines models in the process.

With this in mind, we’ve adopted "API First" as one of our key engineering principles. Microservices development begins with API definition outside the code and ideally involves ample peer-review feedback to achieve high-quality APIs. API First encompasses a set of quality-related standards and fosters a peer review culture including a lightweight review procedure. We encourage our teams to follow them to ensure that our APIs:

- are easy to understand and learn
- are general and abstracted from specific implementation and use cases
- are robust and easy to use
- have a common look and feel
- follow a consistent RESTful style and syntax
- are consistent with other teams’ APIs and our global architecture

Ideally, all BNL APIs will look like the same author created them.

### Conventions used in these guidelines <a id="101"></a>

The requirement level keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" used in this document (case insensitive) are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

### BNL specific information <a id="102"></a>

The purpose of these "RESTful API guidelines" is to define standards to successfully establish "consistent API look and feel" quality. Teams are responsible to fulfill these guidelines during API development and are encouraged to contribute to guideline evolution via pull requests.

These guidelines will, to some extent, remain work in progress as our work evolves, but teams can confidently follow and trust them.

In case guidelines are changing, following rules apply:

- existing APIs don’t have to be changed, but it is recommended to do so
- clients of existing APIs have to cope with these APIs based on outdated rules
- new APIs have to respect the current guidelines

Furthermore you should keep in mind that once an API becomes public externally available, it has to be re-reviewed and changed according to current guidelines - for sake of overall consistency.

## 2. Principles <a id="200"></a>

### API design principles <a id="201"></a>

Comparing SOA web service interfacing style of SOAP vs. REST, the former tend to be centered around operations that are usually use-case specific and specialized. In contrast, REST is centered around business (data) entities exposed as resources that are identified via URIs and can be manipulated via standardized CRUD-like methods using different representations, and hypermedia. RESTful APIs tend to be less use-case specific and comes with less rigid client / server coupling and are more suitable for an ecosystem of (core) services providing a platform of APIs to build diverse new business services. We apply the RESTful web service principles to all kind of application [micro-] service components, independently from whether they provide functionality via the internet or intranet.

- Prefer REST-based APIs with JSON payloads
- Prefer systems to be truly RESTful [[1](#note-1)]

An important principle for API design and usage is Postel’s Law, aka [The Robustness Principle](http://en.wikipedia.org/wiki/Robustness_principle) (see also [RFC 1122](https://tools.ietf.org/html/rfc1122)):

- **Be liberal in what you accept, be conservative in what you send**

### API as a product <a id="202"></a>

As a company BNL wants to deliver products to our (internal and external) customers which can be consumed like a service. Platform products provide their functionality via (public) APIs; hence, the design of BNL's APIs should be based on the API as a Product principle:

- Treat your API as product and act like a product owner
- Put yourself into the place of your customers; be an advocate for their needs
- Emphasize simplicity, comprehensibility, and usability of APIs to make them irresistible for client engineers
- Actively improve and maintain API consistency over the long term
- Make use of customer feedback and provide service level support

Embracing 'API as a Product' facilitates a service ecosystem which can be evolved more easily, and used to experiment quickly with new business ideas by recombining core capabilities. It makes the difference between agile, innovative product service business built on a platform of APIs and ordinary enterprise integration business where APIs are provided as "appendix" of existing products to support system integration and optimised for local server-side realization.

Understand the concrete use cases of your customers and carefully check the trade-offs of your API design variants with a product mindset. Avoid short-term implementation optimizations at the expense of unnecessary client side obligations, and have a high attention on API quality and client developer experience.

### API first <a id="203"></a>

API as a Product is closely related to the API First principle, which is more focused on how we engineer high quality APIs. In a nutshell API First requires two aspects:

- define APIs first, before coding its implementation, using a standard specification language
- get early review feedback from peers and client developers

By defining APIs outside the code, we want to facilitate early review feedback and also a development discipline that focus service interface design on…

- deep understanding of the domain and required functionality
- generalized business entities / resources, i.e. avoidance of use case specific APIs
- clear separation of WHAT vs HOW concerns, i.e. abstraction from implementation aspects — APIs should be stable even if we replace complete service implementation including its underlying technology stack

Moreover, API definitions with standardized specification format also facilitate…

- single source of truth for the API specification; it is a crucial part of a contract between service provider and client users
- infrastructure tooling for API discovery, API GUIs, API documents, automated quality checks

Elements of API First are also this API Guidelines and a standardized API review process as to get early review feedback from peers and client developers. Peer review is important for us to get high quality APIs, to enable architectural and design alignment and to supported development of client applications decoupled from service provider engineering life cycle.

It is important to learn, that API First is **not in conflict with the agile development principles**. Service applications should evolve incrementally — and so its APIs. Of course, API specifications will and should evolve iteratively in different cycles; however, each starting with draft status and *early* team and peer review feedback. API may change and profit from implementation concerns and automated testing feedback. API evolution during development life cycle may include breaking changes for not yet productive features and as long as we have aligned the changes with the clients. Hence, API First does *not* mean that you must have 100% domain and requirement understanding and can never produce code before you have defined the complete API and get it confirmed by peer review. On the other hand, API First obviously is in conflict with the bad practice of publishing API definition and asking for peer review after the service integration or even the service productive operation has started. It is crucial to request and get early feedback — as early as possible, but not before the API changes are comprehensive with focus to the next evolution step and have a certain quality (including API Guideline compliance), already confirmed via team internal reviews.

## 3. General guidelines <a id="300"></a>

The titles are marked with the corresponding labels: MUST, SHOULD, **MAY**.

### MUST follow API first principle <a id="301"></a>

You must follow the API First Principle, more specifically:

- You must define APIs first, before coding its implementation, using Open API as specification language
- You must design your APIs consistently with this guidelines
- You must call for early review feedback from peers and client developers, and apply an API review process for all component external APIs

### MUST provide API specification using Open API <a id="302"></a>

We use the [Open API specification](http://swagger.io/specification/) as standard to define API specification files. API designers are required to provide the API specification using a single **self-contained YAML** file to improve readability. We encourage to use **Open API 3.0** version, but still support **Open API 2.0** (a.k.a. Swagger 2).

The API specification files should be subject to version control using a source code management system - best together with the implementing sources.

You must / should publish] the component external / internal API specifications with the deployment of the implementing service, and, hence.

### MUST only use durable and immutable remote references <a id="303"></a>

Normally, API specification files must be **self-contained**, i.e. files should not contain references to local or remote content, e.g. `../fragment.yaml#/element`. The reason is, that the content referred to is *in general* **not durable** and **not immutable**. As a consequence, the semantic of an API may change in unexpected ways.

### SHOULD provide API user manual <a id="304"></a>

In addition to the API Specification, it is good practice to provide an API user manual to improve client developer experience, especially of engineers that are less experienced in using this API. A helpful API user manual typically describes the following API aspects:

- API scope, purpose, and use cases
- concrete examples of API usage
- edge cases, error situation details, and repair hints
- architecture context and major dependencies - including figures and sequence flows

The user manual must be published online, include a link to the API user manual into the API specification using the `#/externalDocs/url` property.

### MUST write APIs using U.S. English <a id="305"></a>

## 4. Meta information <a id="400"></a>

### MUST contain API meta information <a id="401"></a>

API specifications must contain the following Open API meta information to allow for API management:

- `#/info/title` as (unique) identifying, functional descriptive name of the API
- `#/info/version` to distinguish API specifications versions following [semantic rules](#402)
- `#/info/description` containing a proper description of the API
- `#/info/contact/{name,url,email}` containing the responsible team

Following Open API extension properties MUST be provided in addition:

- `#/info/x-api-id` unique [identifier](#403) of the API 
- `#/info/x-audience` intended [target](#401) audience of the API

### MUST use semantic versioning <a id="402"></a>

Open API allows to specify the API specification version in `#/info/version`. To share a common semantic of version information we expect API designers to comply to [Semantic Versioning 2.0](http://semver.org/spec/v2.0.0.html) rules `1` to `8` and `11` restricted to the format <MAJOR>.<MINOR>.<PATCH> for versions as follows:

- Increment the **MAJOR** version when you make incompatible API changes after having aligned this changes with consumers,
- Increment the **MINOR** version when you add new functionality in a backwards-compatible manner, and
- Optionally increment the **PATCH** version when you make backwards-compatible bug fixes or editorial changes not affecting the functionality.

**Additional Notes:**

- **Pre-release** versions ([rule 9](http://semver.org/#spec-item-9)) and **build metadata** ([rule 10](http://semver.org/#spec-item-10)) must not be used in API version information.
- While patch versions are useful for fixing typos etc, API designers are free to decide whether they increment it or not.
- API designers should consider to use API version `0.y.z` ([rule 4](http://semver.org/#spec-item-4)) for initial API design.

Example:

```yaml
openapi: 3.0.1
info:
  title: Parcel Service API
  description: API for <...>
  version: 1.3.7
```

### MUST provide API identifiers <a id="403"></a>

Each API specification must be provisioned with a globally unique and immutable API identifier. The API identifier is defined in the `info`-block of the Open API specification and must conform to the following definition:

```yaml
/info/x-api-id:
  type: string
  format: urn
  pattern: ^[a-z0-9][a-z0-9-:.]{6,62}[a-z0-9]$
  description: |
    Mandatory globally unique and immutable API identifier. The API
    id allows to track the evolution and history of an API specification
    as a sequence of versions.
```

API specifications will evolve and any aspect of an Open API specification may change. We require API identifiers because we want to support API clients and providers with API lifecycle management features, like change trackability and history or automated backward compatibility checks. The immutable API identifier allows the identification of all API specification versions of an API evolution. By using [API semantic version information](#403) or [API publishing date](#publish) as order criteria you get the **version** or **publication history** as a sequence of API specifications.

**Note**: While it is nice to use human readable API identifiers based on self-managed URNs, it is recommend to stick to UUIDs to relief API designers from any urge of changing the API identifier while evolving the API. Example:

```yaml
openapi: 3.0.1
info:
  x-api-id: d0184f38-b98d-11e7-9c56-68f728c1ba70
  title: Parcel Service API
  description: API for <...>
  version: 1.5.8
```

### MUST provide API audience <a id="404"></a>

Each API must be classified with respect to the intended target **audience** supposed to consume the API, to facilitate differentiated standards on APIs for discoverability, changeability, quality of design and documentation, as well as permission granting. We differentiate the following API audience groups with clear organisational and legal boundaries:

- **component-internal**

  This is often referred to as a *team internal API* or a *product internal API*. The API consumers with this audience are restricted to applications of the same **functional component** which typically represents a specific **product** with clear functional scope and ownership. All services of a functional component / product are owned by a specific dedicated owner and engineering team(s). Typical examples of component-internal APIs are APIs being used by internal helper and worker services or that support service operation.

- **business-unit-internal**

  The API consumers with this audience are restricted to applications of a specific product portfolio owned by the same business unit.

- **company-internal**

  The API consumers with this audience are restricted to applications owned by the business units of the same the company

- **external-partner**

  The API consumers with this audience are restricted to applications of business partners of the company owning the API and the company itself.

- **external-public**

  APIs with this audience can be accessed by anyone with Internet access.

**Note:** a smaller audience group is intentionally included in the wider group and thus does not need to be declared additionally.

The API audience is provided as API meta information in the `info`-block of the Open API specification and must conform to the following specification:

```yaml
/info/x-audience:
  type: string
  x-extensible-enum:
    - component-internal
    - business-unit-internal
    - company-internal
    - external-partner
    - external-public
  description: |
    Intended target audience of the API. Relevant for standards around
    quality of design and documentation, reviews, discoverability,
    changeability, and permission granting.
```

**Note:** Exactly **one audience** per API specification is allowed. For this reason a smaller audience group is intentionally included in the wider group and thus does not need to be declared additionally. If parts of your API have a different target audience, we recommend to split API specifications along the target audience — even if this creates redundancies

Example:

```yaml
openapi: 3.0.1
info:
  x-audience: company-internal
  title: Parcel Helper Service API
  description: API for <...>
  version: 1.2.4
  <...>
```

## 5. Security <a id="500"></a>

### MUST secure endpoints <a id="501"></a>

Every API endpoint needs to be secured using `[HTTP Authorization Headers {Basic, Bearer} | API Keys | OAuth 2.0 | OpenID Connect]`. Please refer to the [Authentication section](https://swagger.io/docs/specification/authentication/) of the official Open API specification on how to specify security definitions in your API.

The following code snippet shows how to define the authorization scheme using Basic Authentication

```yaml
openapi: 3.0.0
...
components:
  securitySchemes:
    basicAuth:     # <-- arbitrary name for the security scheme
      type: http
      scheme: basic
security:
  - basicAuth: []  # <-- use the same name here
```

### MUST define and assign permissions (scopes) <a id="502"></a>

APIs must define permissions to protect their resources. Thus, at least one permission must be assigned to each endpoint.

Please refer to [MUST follow naming convention for permissions (scopes)](#503) for designing permission names.

APIs should stick to component specific permissions without resource extension to avoid governance complexity of too many fine grained permissions. For the majority of use cases, restricting access to specific API endpoints using read and write is sufficient for controlling access for client types like business partners, customers or operational staff. However, in some situations, where the API serves different types of resources for different owners, resource specific scopes may make sense.

Some examples for standard and resource-specific permissions:

| Application ID             | Resource ID      | Access Type | Example                                |
| :------------------------- | :--------------- | :---------- | :------------------------------------- |
| `new_ambition`             | `subscription`   | `read`      | `new_ambition.subscription.read`       |
| `busness-partner-a`        | `quotation`      | `write`     | `busness-partner-a.quotation.write`    |

After permission names are defined and the permission is declared in the security definition at the top of an API specification, it should be assigned to each API operation by specifying a [security requirement](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#securityRequirementObject) like this:

```yaml
paths:
 /subscriptions/{id}:
    get:
      summary: Retrieves information about a product subscription
      security:
        - basicAuth: [ subscription.read ]
```

Hint: you need not explicitly define the "Authorization" header; it is a standard header so to say implicitly defined via the security section.

## 6. Compatibility <a id="600"></a>

### MUST not break backward compatibility <a id="601"></a>

Change APIs, but keep all consumers running. Consumers usually have independent release lifecycles, focus on stability, and avoid changes that do not provide additional value. APIs are contracts between service providers and service consumers that cannot be broken via unilateral decisions.

So, to change APIs without breaking them, introduce new API versions and still support older versions.

### SHOULD design APIs conservatively <a id="602"></a>

Designers of service provider APIs should be conservative and accurate in what they accept from clients:

- Unknown input fields in payload or URL should not be ignored; servers should provide error feedback to clients via an HTTP 400 response code.

- Be accurate in defining input data constraints (like formats, ranges, lengths etc.) — and check constraints and return dedicated error information in case of violations.

### MUST always return JSON objects as top-level data structures <a id="603"></a>

In a response body, you must always return a JSON object (and not e.g. an array) as a top level data structure to support future extensibility. JSON objects support compatible extension by additional attributes. This allows you to easily extend your response and e.g. add pagination later, without breaking backwards compatibility.

## 7. Deprecation <a id="700"></a>

Sometimes it is necessary to phase out an API endpoint, an API version, or an API feature, e.g. if a field or parameter is no longer supported or a whole business functionality behind an endpoint is supposed to be shut down. As long as the API endpoints and features are still used by consumers these shut downs are breaking changes and not allowed. To progress the following deprecation rules have to be applied to make sure that the necessary consumer changes and actions are well communicated and aligned using deprecation and sunset dates.

### MUST obtain approval of clients before API shut down <a id="701"></a>

Before shutting down an API, version of an API, or API feature the producer must make sure, that all clients have given their consent on a sunset date. Producers should help consumers to migrate to a potential new API or API feature by providing a migration manual and clearly state the time line for replacement availability and sunset (see also [SHOULD add Deprecation and Sunset header to responses](#705)). Once all clients of a sunset API feature are migrated, the producer may shut down the deprecated API feature.

### MUST collect external partner consent on deprecation time span <a id="702"></a>

If the API is consumed by any external partner, the API owner must define a reasonable time span that the API will be maintained after the producer has announced deprecation. All external partners must state consent with this after-deprecation-life-span, i.e. the minimum time span between official deprecation and first possible sunset, before they are allowed to use the API.

### MUST reflect deprecation in API specifications <a id="703"></a>

The API deprecation must be part of the API specification.

If an API endpoint (operation object), an input argument (parameter object), an in/out data object (schema object), or on a more fine grained level, a schema attribute or property should be deprecated, the producers must set deprecated: true for the affected element and add further explanation to the description section of the API specification. If a future shut down is planned, the producer must provide a sunset date and document in details what consumers should use instead and how to migrate.

### MUST monitor usage of deprecated API scheduled for sunset <a id="704"></a>

Owners of an API, API version, or API feature used in production that is scheduled for sunset must monitor the usage of the sunset API, API version, or API feature in order to observe migration progress and avoid uncontrolled breaking effects on ongoing consumers. See also [SHOULD monitor API usage](#TODO).

### SHOULD add `Deprecation` and `Sunset` header to responses <a id="705"></a>

During the deprecation phase, the producer should add a `Deprecation: <date-time>` (see draft: [RFC Deprecation HTTP Header](https://tools.ietf.org/html/draft-dalal-deprecation-header)) and - if also planned - a `Sunset: <date-time>` (see [RFC 8594](https://tools.ietf.org/html/rfc8594#section-3)) header on each response affected by a deprecated element (see [MUST reflect deprecation in API specifications](#703)).

The `Deprecation` header can either be set to `true` - if a feature is retired -, or carry a deprecation time stamp, at which a replacement will become/became available and consumers must not on-board any longer (see [MUST not start using deprecated APIs](#707)). The optional Sunset time stamp carries the information when consumers latest have to stop using a feature. The sunset date should always offer an eligible time interval for switching to a replacement feature.

```
Deprecation: Tue, 31 Dec 2024 23:59:59 GMT
Sunset: Wed, 31 Dec 2025 23:59:59 GMT
```

If multiple elements are deprecated the `Deprecation` and `Sunset` headers are expected to be set to the earliest time stamp to reflect the shortest interval consumers are expected to get active.

**Note**: adding the `Deprecation` and `Sunset` header is not sufficient to gain client consent to shut down an API or feature.

### SHOULD add monitoring for Deprecation and Sunset header <a id="706"></a>

Clients should monitor the `Deprecation` and `Sunset` headers in HTTP responses to get information about future sunset of APIs and API features (see [SHOULD add Deprecation and Sunset header to responses](#705)). We recommend that client owners build alerts on this monitoring information to ensure alignment with service owners on required migration task.

### MUST not start using deprecated APIs <a id="707"></a>

Clients must not start using deprecated APIs, API versions, or API features.

## 8. JSON guidelines <a id="800"></a>

These guidelines provides recommendations for defining JSON data at BNL. JSON here refers to [RFC 7159](https://tools.ietf.org/html/rfc7159) (which updates [RFC 4627](https://tools.ietf.org/html/rfc4627)), the "application/json" media type and custom JSON media types defined for APIs. The guidelines clarifies some specific cases to allow BNL JSON data to have an idiomatic form across teams and services.

The first some of the following guidelines are about property names, the later ones about values.

### MUST property names must be ASCII snake_case (and never camelCase): `^[a-z_][a-z_0-9]*$` <a id="801"></a>

Property names are restricted to ASCII strings. The first character must be a letter, or an underscore, and subsequent characters can be a letter, an underscore, or a number.

(It is recommended to use `_` at the start of property names only for keywords like `_links`.)

Rationale: No established industry standard exists, but many popular Internet companies prefer snake_case: e.g. GitHub, Stack Exchange, Twitter. Others, like Google and Amazon, use both - but not only camelCase. It’s essential to establish a consistent look and feel such that JSON looks as if it came from the same hand.

### SHOULD represent enumerations as strings <a id="802"></a>

Strings are a reasonable target for values that are by design enumerations.

### SHOULD declare enum values using UPPER_SNAKE_CASE format <a id="803"></a>

Enum values need to consistently use the upper-snake case format, e.g. `VALUE` or `YET_ANOTHER_VALUE`. This approach allows to clearly distinguish values from properties or other elements.

Note: This does not apply where the actual exact values are coming from some outside source, e.g. for language codes from [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes), or when declaring possible values for a `sort parameter`. # TODO reference

### SHOULD define maps using `additionalProperties` <a id="804"></a>

A "map" here is a mapping from string keys to some other type. In JSON this is represented as an object, the key-value pairs being represented by property names and property values. In Open API schema (as well as in JSON schema) they should be represented using additionalProperties with a schema defining the value type. Such an object should normally have no other defined properties.

The map keys don’t count as property names in the sense of rule "[MUST property names must be ASCII snake_case](#801)", and can follow whatever format is natural for their domain. Please document this in the description of the map object’s schema.

Here is an example for such a map definition (the `translations` property):

```yaml
components:
  schemas:
    Message:
      description:
        A message together with translations in several languages.
      type: object
      properties:
        message_key:
          type: string
          description: The message key.
        translations:
          description:
            The translations of this message into several languages.
            The keys are [IETF BCP-47 language tags](https://tools.ietf.org/html/bcp47).
          type: object
          additionalProperties:
            type: string
            description:
              the translation of this message into the language identified by the key.
```

An actual JSON object described by this might then look like this:

```json
{ 
  "message_key": "color",
  "translations": {
    "de": "Farbe",
    "en-US": "color",
    "en-GB": "colour",
    "eo": "koloro",
    "nl": "kleur"
  }
}
```

### SHOULD pluralize array names <a id="805"></a>

To indicate they contain multiple values prefer to pluralize array names. This implies that object names should in turn be singular.

### MUST not use null for boolean properties <a id="806"></a>

Schema based JSON properties that are by design booleans must not be presented as nulls. A boolean is essentially a closed enumeration of two values, true and false. If the content has a meaningful null value, strongly prefer to replace the boolean with enumeration of named values or statuses - for example accepted_terms_and_conditions with true or false can be replaced with terms_and_conditions with values yes, no and unknown.

### MUST use same semantics for null and absent properties <a id="807"></a>

Open API 3.x allows to mark properties as required and as nullable to specify whether properties may be absent ({}) or null ({"example":null}). If a property is defined to be not required and nullable (see 2nd row in Table below), this rule demands that both cases must be handled in the exact same manner by specification.

The following table shows all combinations and whether the examples are valid:

| required | nullable | {}    | {"example":null} |
|----------|----------|-------|------------------|
| true     | true     | ✗ No  | ✔ Yes            |
| false    | true     | ✔ Yes | ✔ Yes            |
| true     | false    | ✗ No  | ✗ No             |
| false    | false    | ✔ Yes | ✗ No             |

While API designers and implementers may be tempted to assign different semantics to both cases, we explicitly decide **against** that option, because we think that any gain in expressiveness is far outweighed by the risk of clients not understanding and implementing the subtle differences incorrectly.

As an example, an API that provides the ability for different users to coordinate on a time schedule, e.g. a meeting, may have a resource for options in which every user has to make a **choice**. The difference between _undecided_ and _decided against any of the options_ could be modeled as _absent_ and `null` respectively. It would be safer to express the `null` case with a dedicated [Null object](https://en.wikipedia.org/wiki/Null_object_pattern), e.g. `{}` compared to `{"id":"42"}`.

Moreover, many major libraries have somewhere between little to no support for a `null`/absent pattern (see [Gson](https://stackoverflow.com/questions/48465005/gson-distinguish-null-value-field-and-missing-field), [Moshi](https://github.com/square/moshi#borrows-from-gson), [Jackson](https://github.com/FasterXML/jackson-databind/issues/578), [JSON-B](https://developer.ibm.com/articles/j-javaee8-json-binding-3/)). Especially strongly-typed languages suffer from this since a new composite type is required to express the third state. Nullable `Option/Optional/Maybe` types could be used but having nullable references of these types completely contradicts their purpose.

The only exception to this rule is JSON Merge Patch [RFC 7396](https://tools.ietf.org/html/rfc7396) which uses null to explicitly indicate property deletion while absent properties are ignored, i.e. not modified.

### SHOULD not use null for empty arrays <a id="808"></a>

Empty array values can unambiguously be represented as the empty list, `[]`.

### SHOULD name date/time properties with `_at` suffix <a id="809"></a>

Dates and date-time properties should end with _at to distinguish them from boolean properties which otherwise would have very similar or even identical names:

- `created_at` rather than `created`,
- `modified_at` rather than `modified`,
- `occurred_at` rather than `occurred`, and
- `returned_at` rather than `returned`.

### SHOULD define dates properties compliant with RFC 3339 <a id="810"></a>

Use the date and time formats defined by [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6):

- for "date" use strings matching `date-fullyear "-" date-month "-" date-mday`, for example: `2015-05-28`
- for "date-time" use strings matching `full-date "T" full-time`, for example `2015-05-28T14:07:17Z`

Note that the Open API format "date-time" corresponds to "date-time" in the RFC) and `2015-05-28` for a date (note that the Open API format "date" corresponds to "full-date" in the RFC). Both are specific profiles, a subset of the international standard ISO 8601.

A zone offset may be used (both, in request and responses) - this is simply defined by the standards. However, we encourage restricting dates to UTC and without offsets. For example `2015-05-28T14:07:17Z` rather than `2015-05-28T14:07:17+00:00`. From experience we have learned that zone offsets are not easy to understand and often not correctly handled. Note also that zone offsets are different from local times that might be including daylight saving time. Localization of dates should be done by the services that provide user interfaces, if required.

When it comes to storage, all dates should be consistently stored in UTC without a zone offset. Localization should be done locally by the services that provide user interfaces, if required.

Sometimes it can seem data is naturally represented using numerical timestamps, but this can introduce interpretation issues with precision, e.g. whether to represent a timestamp as 1460062925, 1460062925000 or 1460062925.000. Date strings, though more verbose and requiring more effort to parse, avoid this ambiguity.

### SHOULD define time durations and intervals properties conform to ISO 8601 <a id="811"></a>

Schema based JSON properties that are by design durations and intervals could be strings formatted as recommended by [ISO 8601(Appendix A of RFC 3339 contains a grammar for durations)](https://tools.ietf.org/html/rfc3339#appendix-A).

## 9. Data formats <a id="900"></a>

### MUST use JSON to encode structured data <a id="901"></a>

Use JSON-encoded body payload for transferring structured data. The JSON payload must follow [RFC 7159](https://tools.ietf.org/html/rfc7159) using a JSON object as top-level data structure (if possible) to allow for future extension. This also applies to collection resources, where one naturally would assume an array. See also [MUST always return JSON objects as top-level data structures](#603).

Additionally, the JSON payload must comply to [RFC 7493](https://tools.ietf.org/html/rfc7493), particularly

- [Section 2.1](https://tools.ietf.org/html/rfc7493#section-2.1) on encoding of characters, and
- [Section 2.3](https://tools.ietf.org/html/rfc7493#section-2.3) on object constraints.

As a consequence, a JSON payload must

- use `UTF-8` [encoding](https://tools.ietf.org/html/rfc7493#section-2.1)
- consist of [valid Unicode strings](https://tools.ietf.org/html/rfc7493#section-2.1), i.e. must not contain non-characters or surrogates, and
- contain only [unique member names](https://tools.ietf.org/html/rfc7493#section-2.3) (no duplicate names).

### MAY use non JSON media types for binary data or alternative content representations <a id="902"></a>

Other media types may be used in following cases:

- transferring binary data or data whose structure is not relevant. This is the case if payload structure is not interpreted and consumed by clients as is. Example of such use case is downloading images in formats JPG, PNG, GIF.
- in addition to JSON version alternative data representations (e.g. in formats PDF, DOC, XML) may be made available through content negotiation.

### SHOULD encode embedded binary data in base64url <a id="903"></a>

Exposing binary data using an alternative media type is generally preferred. See [MAY use non JSON media types for binary data or alternative content representations](#902).

If an alternative content representation is not desired then binary data should be embedded into the JSON document as a base64url-encoded string property following [RFC 7493 Section 4.4](https://tools.ietf.org/html/rfc7493#section-4.4).

### MUST define format for number and integer types <a id="904"></a>

Whenever an API defines a property of type `number` or `integer`, the precision must be defined by the format as follows to prevent clients from guessing the precision incorrectly, and thereby changing the value unintentionally:

| type    | format  | specified value range                                |
|---------|---------|------------------------------------------------------|
| integer | int32   | integer between -231 and 231-1                       |
| integer | int64   | integer between -263 and 263-1                       |
| integer | bigint  | arbitrarily large signed integer number              |
| number  | float   | [IEEE 754-2008/ISO 60559:2011](https://en.wikipedia.org/wiki/IEEE_754) binary32 decimal number |
| number  | double  | [IEEE 754-2008/ISO 60559:2011](https://en.wikipedia.org/wiki/IEEE_754) binary64 decimal number |
| number  | decimal | arbitrarily precise signed decimal number            |

The precision must be translated by clients and servers into the most specific language types. E.g. for the following definitions the most specific language types in Java will translate to `BigDecimal` for `Money.amount` and `int` or `Integer` for the `OrderList.page_size`:

```yaml
components:
  schemas:
    Money:
      type: object
      properties:
        amount:
          type: number
          description: Amount expressed as a decimal number of major currency units
          format: decimal
          example: 99.95
       ...

    OrderList:
      type: object
      properties:
        page_size:
          type: integer
          description: Number of orders in list
          format: int32
          example: 42
```

## 10. Common data types <a id="1000"></a>

### MUST use common field names and semantics <a id="1001"></a>

There exist a variety of field types that are required in multiple places. To achieve consistency across all API implementations, you must use common field names and semantics whenever applicable.

#### Generic fields

There are some data fields that come up again and again in API data:
- `id`: the identity of the object. If used, IDs must be opaque strings and not numbers. IDs are unique within some documented context, are stable and don’t change for a given object once assigned, and are never recycled cross entities.
- `xyz_id`: an attribute within one object holding the identifier of another object must use a name that corresponds to the type of the referenced object or the relationship to the referenced object followed by `_id` (e.g. `partner_id` not `partner_number`, or `parent_node_id` for the reference to a parent node from a child node, even if both have the type `Node`).
- `created_at`: when the object was created. If used, this must be a `date-time` construct
- `modified_at`: when the object was updated.
- `type`: the kind of thing this object is. If used, the type of this field should be a string. Types allow runtime information on the entity provided that otherwise requires examining the Open API file.
- `ETag`: the [ETag](#TODO) of an [embedded sub-resource](#TODO). It may be used to carry the `ETag` for subsequent `PUT`/`PATCH` calls (see [ETags in result entities](#TODO)).

Example JSON schema:

```yaml
tree_node:
  type: object
  properties:
    id:
      description: the identifier of this node
      type: string
    created_at:
      description: when got this node created
      type: string
      format: 'date-time'
    modified_at:
      description: when got this node last updated
      type: string
      format: 'date-time'
    type:
      type: string
      enum: [ 'LEAF', 'NODE' ]
    parent_node_id:
      description: the identifier of the parent node of this node
      type: string
  example:
    id: '123435'
    created_at: '2017-04-12T23:20:50.52Z'
    modified_at: '2017-04-12T23:20:50.52Z'
    type: 'LEAF'
    parent_node_id: '534321'
```

These properties are not always strictly necessary, but making them idiomatic allows API client developers to build up a common understanding of BNL’s resources. There is very little utility for API consumers in having different names or value types for these fields across APIs.

#### Link relation fields

To foster a consistent look and feel using simple hypertext controls for paginating and iterating over collection values the response objects should follow a common pattern using the below field semantics:

- `self`: the link or cursor in a pagination response or object pointing to the same collection object or page.
- `first`: the link or cursor in a pagination response or object pointing to the first collection object or page.
- `prev`: the link or cursor in a pagination response or object pointing to the previous collection object or page.
- `next`: the link or cursor in a pagination response or object pointing to the next collection object or page.
- `last`: the link or cursor in a pagination response or object pointing to the last collection object or page.

Pagination responses should contain the following additional array field to transport the page content:

- `items`: array of resources, holding all the items of the current page (items may be replaced by a resource name).

To simplify the user experience, the applied query filters may be returned using the following field:

- `query`: object containing the query filters applied in the search request to filter the collection resource.

As Result, the standard response page using [pagination links](#TODO) is defined as follows:

```yaml
ResponsePage:
  type: object
  properties:
    self:
      description: Pagination link pointing to the current page.
      type: string
      format: uri
    first:
      description: Pagination link pointing to the first page.
      type: string
      format: uri
    prev:
      description: Pagination link pointing to the previous page.
      type: string
      format: uri
    next:
      description: Pagination link pointing to the next page.
      type: string
      format: uri
    last:
      description: Pagination link pointing to the last page.
      type: string
      format: uri

     query:
       description: >
         Object containing the query filters applied to the collection resource.
       type: object
       properties: ...

     items:
       description: Array of collection items.
       type: array
       required: false
       items:
         type: ...
```

The response page may contain additional metadata about the collection or the current page.

## 11. API naming <a id="1100"></a>

### MUST use lowercase separate words with hyphens for path segments <a id="1101"></a>

Example:

```http request
/shipment-orders/{shipment-order-id}
```

This applies to concrete path segments and not the names of path parameters. For example `{shipment_order_id}` would be ok as a path parameter.

### MUST use snake_case (never camelCase) for query parameters <a id="1102"></a>

Examples:

```
customer_number, order_id, billing_address
```

### SHOULD prefer hyphenated-pascal-case for HTTP header fields <a id="1103"></a>

This is for consistency in your documentation (most other headers follow this convention). Avoid camelCase (without hyphens). Exceptions are common abbreviations like "ID."

Examples:

```http request
Accept-Encoding
Apply-To-Redirect-Ref
Disposition-Notification-Options
Original-Message-ID
```

See also: [HTTP Headers are case-insensitive (RFC 7230)](#TODO).

See [Common headers](#TODO) and [Proprietary headers](#TODO) sections for more guidance on HTTP headers.

### MUST pluralize resource names <a id="1104"></a>

Usually, a collection of resource instances is provided (at least the API should be ready here). The special case of a resource singleton must be modeled as a collection with cardinality 1 including definition of `maxItems` = `minItems` = 1 for the returned `array` structure to make the cardinality constraint explicit.

**Exception**: the pseudo identifier `self` used to specify a resource endpoint where the resource identifier is provided by authorization information (see [MUST identify resources and sub-resources via path segments](#TODO)).

### SHOULD not use /api as base path <a id="1105"></a>

In most cases, all resources provided by a service are part of the public API, and therefore should be made available under the root "/" base path.

If the service should also support non-public, internal APIs — for specific operational support functions, for example — we encourage you to maintain two different API specifications and provide [API audience](#404). For both APIs, you should not use /api as base path.

We see API’s base path as a part of deployment variant configuration. Therefore, this information has to be declared in the [server object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md#server-object).

### MUST use normalized paths without empty path segments and trailing slashes <a id="1106"></a>

You must not specify paths with duplicate or trailing slashes, e.g. `/customers//addresses` or `/customers/`. As a consequence, you must also not specify or use path variables with empty string values.

**Reasoning**: Non standard paths have no clear semantics. As a result, behavior for non standard paths varies between different HTTP infrastructure components and libraries. This may leads to ambiguous and unexpected results during request handling and monitoring.

We recommend to implement services robust against clients not following this rule. All services SHOULD [normalize](https://en.wikipedia.org/wiki/URI_normalization) request paths before processing by removing duplicate and trailing slashes. Hence, the following requests should refer to the same resource:

```http request
GET /orders/{order-id}
GET /orders/{order-id}/
GET /orders//{order-id}
```

**Note**: path normalization is not supported by all frameworks out-of-the-box. Services are required to support at least the normalized path while rejecting all alternatives paths, if failing to deliver the same resource.

### MUST stick to conventional query parameters <a id="1107"></a>

If you provide query support for searching, sorting, filtering, and paginating, you must stick to the following naming conventions:
- `q`: default query parameter, e.g. used by browser tab completion; should have an entity specific alias, e.g. sku.
- `sort`: comma-separated list of fields (as defined by [MUST define collection format of header and query parameters](#TODO)) to define the sort order. To indicate sorting direction, fields may be prefixed with `+` (ascending) or `-` (descending), e.g. `/sales-orders?sort=+id.`
- `fields`: field name expression to retrieve only a subset of fields of a resource. See [SHOULD support partial responses via filtering](#TODO) below.
- `embed`: field name expression to expand or embedded sub-entities, e.g. inside of an article entity, expand silhouette code into the silhouette object. Implementing `embed` correctly is difficult, so do it with care. See S[HOULD allow optional embedding of sub-resources below](#TODO).
- `offset`: numeric offset of the first element provided on a page representing a collection request. See [Pagination](#TODO) section below.
- `cursor`: an opaque pointer to a page, never to be inspected or constructed by clients. It usually (encrypted) encodes the page position, i.e. the identifier of the first or last page element, the pagination direction, and the applied query filters to recreate the collection. See [Pagination](#TODO) section below.

limit: client suggested limit to restrict the number of entries on a page. See [Pagination](#TODO) section below.

## 12. Resources <a id="1200"></a>

### SHOULD avoid actions — think about resources <a id="1201"></a>

REST is all about your resources, so consider the domain entities that take part in web service interaction, and aim to model your API around these using the standard HTTP methods as operation indicators. For instance, if an application has to lock articles explicitly so that only one user may edit them, create an article lock with `PUT` or `POST` instead of using a lock action.

Request:

```http request
PUT /article-locks/{article-id}
```

The added benefit is that you already have a service for browsing and filtering article locks.

### SHOULD model complete business processes <a id="1202"></a>

An API should contain the complete business processes containing all resources representing the process. This enables clients to understand the business process, foster a consistent design of the business process, allow for synergies from description and implementation perspective, and eliminates implicit invisible dependencies between APIs.

In addition, it prevents services from being designed as thin wrappers around databases, which normally tends to shift business logic to the clients.


### SHOULD define useful resources <a id="1203"></a>

As a rule of thumb resources should be defined to cover 90% of all its client’s use cases. A useful resource should contain as much information as necessary, but as little as possible. A great way to support the last 10% is to allow clients to specify their needs for more/less information by supporting filtering and [embedding](#TODO)

### MUST keep URLs verb-free <a id="1204"></a>

The API describes resources, so the only place where actions should appear is in the HTTP methods. In URLs, use only nouns. Instead of thinking of actions (verbs), it’s often helpful to think about putting a message in a letter box: e.g., instead of having the verb cancel in the url, think of sending a message to cancel an order to the cancellations letter box on the server side.

### MUST use domain-specific resource names <a id="1205"></a> 

API resources represent elements of the application’s domain model. Using domain-specific nomenclature for resource names helps developers to understand the functionality and basic semantics of your resources. It also reduces the need for further documentation outside the API definition. For example, "sales-order-items" is superior to "order-items" in that it clearly indicates which business object it represents. Along these lines, "items" is too general.

### MUST use URL-friendly resource identifiers: [a-zA-Z0-9:._\-/]* <a id="1206"></a>

To simplify encoding of resource IDs in URLs, their representation must only consist of ASCII strings using letters, numbers, underscore, minus, colon, period, and - on rare occasions - slash.

**Note**: slashes are only allowed to build and signal resource identifiers consisting of [compound keys](#TODO).

**Note**: to prevent ambiguities of [unnormalized paths](#TODO) resource identifiers must never be empty. Consequently, support of empty strings for path parameters is forbidden.

### MUST identify resources and sub-resources via path segments <a id="1207"></a>

Some API resources may contain or reference sub-resources. Embedded sub-resources, which are not top-level resources, are parts of a higher-level resource and cannot be used outside of its scope. Sub-resources should be referenced by their name and identifier in the path segments as follows:

```http request
/resources/{resource-id}/sub-resources/{sub-resource-id}
```

In order to improve the consumer experience, you should aim for intuitively understandable URLs, where each sub-path is a valid reference to a resource or a set of resources. For instance, if `/partners/{partner-id}/addresses/{address-id}` is valid, then, in principle, also `/partners/{partner-id}/addresses`, `/partners/{partner-id}` and `/partners` must be valid. Examples of concrete url paths:

```http request
/shopping-carts/de:1681e6b88ec1/items/1
/shopping-carts/de:1681e6b88ec1
/content/images/9cacb4d8
/content/images
```

### SHOULD only use UUIDs if necessary <a id="1208"></a>

Generating IDs can be a scaling problem in high frequency and near real time use cases. UUIDs solve this problem, as they can be generated without collisions in a distributed, non-coordinated way and without additional server round trips.

However, they also come with some disadvantages:
- pure technical key without meaning; not ready for naming or name scope conventions that might be helpful for pragmatic reasons, e.g. we learned to use names for product attributes, instead of UUIDs
- cannot be memorized and easily communicated by humans
- harder to use in debugging and logging analysis
- less convenient for consumer facing usage
- quite long: readable representation requires 36 characters and comes with higher memory and bandwidth consumption
- not ordered along their creation history and no indication of used id volume
- may be in conflict with additional backward compatibility support of legacy ids

UUIDs should be avoided when not needed for large scale id generation. Instead, for instance, server side support with id generation can be preferred (`POST` on id resource, followed by idempotent `PUT` on entity resource). Usage of UUIDs is especially discouraged as primary keys of master and configuration data, like brand-ids or attribute-ids which have low id volume but widespread steering functionality.

Please be aware that sequential, strictly monotonically increasing numeric identifiers may reveal critical, confidential business information, like order volume, to non-privileged clients.

In any case, we should always use string rather than number type for identifiers. This gives us more flexibility to evolve the identifier naming scheme. Accordingly, if used as identifiers, UUIDs should not be qualified using a format property.

Hint: Usually, random UUID is used - see UUID version 4 in [RFC 4122](https://tools.ietf.org/html/rfc4122). Though UUID version 1 also contains leading timestamps it is not reflected by its lexicographic sorting. This deficit is addressed by [ULID](https://github.com/alizain/ulid) (Universally Unique Lexicographically Sortable Identifier). You may favour ULID instead of UUID, for instance, for pagination use cases ordered along creation time.

### SHOULD limit number of resource types <a id="1209"></a>

To keep maintenance and service evolution manageable, we should follow "functional segmentation" and "separation of concern" design principles and do not mix different business functionalities in same API definition. In practice this means that the number of resource types exposed via an API should be limited. In this context a resource type is defined as a set of highly related resources such as a collection, its members and any direct sub-resources.

For example, the resources below would be counted as three resource types, one for customers, one for the addresses, and one for the customers' related addresses:

```http request
/customers
/customers/{id}
/customers/{id}/preferences
/customers/{id}/addresses
/customers/{id}/addresses/{addr}
/addresses
/addresses/{addr}
```

Note that:
- We consider `/customers/id/preferences` part of the `/customers` resource type because it has a one-to-one relation to the customer without an additional identifier.
- We consider `/customers` and `/customers/id/addresses` as separate resource types because `/customers/id/addresses/{addr}` also exists with an additional identifier for the address.
- We consider `/addresses` and \ as separate resource types because there’s no reliable way to be sure they are the same.

Given this definition, our experience is that well defined APIs involve no more than 4 to 8 resource types. There may be exceptions with more complex business domains that require more resources, but you should first check if you can split them into separate subdomains with distinct APIs.

Nevertheless one API should hold all necessary resources to model complete business processes helping clients to understand these flows.

### SHOULD limit number of sub-resource levels <a id="1210"></a>

There are main resources (with root url paths) and sub-resources (or nested resources with non-root urls paths). Use sub-resources if their life cycle is (loosely) coupled to the main resource, i.e. the main resource works as collection resource of the subresource entities. You should use ⇐ 3 sub-resource (nesting) levels - more levels increase API complexity and url path length. (Remember, some popular web browsers do not support URLs of more than 2000 characters.)

## 13. HTTP requests <a id="1300"></a>

### MUST use HTTP methods correctly <a id="1301"></a>

Be compliant with the standardized HTTP method semantics summarized as follows:

#### GET

`GET` requests are used to **read** either a single or a collection resource.
- `GET` requests for individual resources will usually generate a `404` if the resource does not exist
- `GET` requests for collection resources may return either `200` (if the collection is empty) or `404` (if the collection is missing)
- `GET` requests must NOT have a request body payload (see `GET with body`)

**Note**: `GET` requests on collection resources should provide sufficient [filter](#TODO) and [Pagination](#TODO) mechanisms.

#### GET with body

APIs sometimes face the problem, that they have to provide extensive structured request information with `GET`, that may conflict with the size limits of clients, load-balancers, and servers. As we require APIs to be standard conform (body in `GET` must be ignored on server side), API designers have to check the following two options:

1. `GET` with URL encoded query parameters: when it is possible to encode the request information in query parameters, respecting the usual size limits of clients, gateways, and servers, this should be the first choice. The request information can either be provided via multiple query parameters or by a single structured URL encoded string.

2. `POST` with body content: when a `GET` with URL encoded query parameters is not possible, a POST with body content must be used. In this case the endpoint must be documented with the hint `GET with body` to transport the `GET` semantic of this call.

**Note**: It is no option to encode the lengthy structured request information using header parameters. From a conceptual point of view, the semantic of an operation should always be expressed by the resource names, as well as the involved path and query parameters. In other words by everything that goes into the URL. Request headers are reserved for general context information. In addition, size limits on query parameters and headers are not reliable and depend on clients, gateways, server, and actual settings. Thus, switching to headers does not solve the original problem.

**Hint**: As `GET with body` is used to transport extensive query parameters, the cursor cannot any longer be used to encode the query filters in case of [cursor-based pagination](#TODO). As a consequence, it is best practice to transport the query filters in the body, while using [pagination links](#TODO) containing the cursor that is only encoding the page position and direction. To protect the pagination sequence the cursor may contain a hash over all applied query filters (See also [SHOULD use pagination links where applicable](#TODO)).

#### PUT 

PUT requests are used to update (in rare cases to create) entire resources – single or collection resources. The semantic is best described as "_please put the enclosed representation at the resource mentioned by the URL, replacing any existing resource._".

- `PUT` requests are usually applied to single resources, and not to collection resources, as this would imply replacing the entire collection
- `PUT` requests are usually robust against non-existence of resources by implicitly creating before updating
- on successful `PUT` requests, the server will **replace the entire resource** addressed by the URL with the representation passed in the payload (subsequent reads will deliver the same payload)
- successful `PUT` requests will usually generate `200` or `204` (if the resource was updated – with or without actual content returned), and `201` (if the resource was created)

**Important**: It is best practice to prefer `POST` over `PUT` for creation of (at least top-level) resources. This leaves the resource ID under control of the service and allows to concentrate on the update semantic using `PUT` as follows.

**Note**: In the rare cases where `PUT` is although used for resource creation, the resource IDs are maintained by the client and passed as a URL path segment. Putting the same resource twice is required to be [idempotent](#TODO) and to result in the same single resource instance (see [MUST fulfill common method properties](#TODO)).

**Hint**: To prevent unnoticed concurrent updates and duplicate creations when using `PUT`, you MAY consider to support `ETag` together with `If-Match`/`If-None-Match` header to allow the server to react on stricter demands that expose conflicts and prevent lost updates. See also [Optimistic locking in RESTful APIs](#TODO) for details and options.

#### POST

`POST` requests are idiomatically used to **create** single resources on a collection resource endpoint, but other semantics on single resources endpoint are equally possible. The semantic for collection endpoints is best described as "_please add the enclosed representation to the collection resource identified by the URL_".

- on a successful `POST` request, the server will create one or multiple new resources and provide their URI/URLs in the response
- successful `POST` requests will usually generate `200` (if resources have been updated), `201` (if resources have been created), `202` (if the request was accepted but has not been finished yet), and exceptionally `204` with `Location` header (if the actual resource is not returned).

The semantic for single resource endpoints is best described as "_please execute the given well specified request on the resource identified by the URL_".

**Generally**: `POST` should be used for scenarios that cannot be covered by the other methods sufficiently. In such cases, make sure to document the fact that `POST` is used as a workaround (see `GET with body`).

**Note**: Resource IDs with respect to `POST` requests are created and maintained by server and returned with response payload.

**Hint**: Posting the same resource twice is not required to be idempotent (check[ MUST fulfill common method properties](#TODO)) and may result in multiple resources. However, you [SHOULD consider to design POST and PATCH idempotent](#TODO) to prevent this.

#### PATCH

`PATCH` requests are used to **update parts** of single resources, i.e. where only a specific subset of resource fields should be replaced. The semantic is best described as "_please change the resource identified by the URL according to my change request_". The semantic of the change request is not defined in the HTTP standard and must be described in the API specification by using suitable media types.

- `PATCH` requests are usually applied to single resources as patching entire collection is challenging
- `PATCH` requests are usually not robust against non-existence of resource instances
- on successful `PATCH` requests, the server will update parts of the resource addressed by the URL as defined by the change request in the payload
- successful `PATCH` requests will usually generate `200` or `204` (if resources have been updated with or without updated content returned)

**Note**: since implementing `PATCH` correctly is a bit tricky, we strongly suggest to choose one and only one of the following patterns per endpoint, unless forced by a [backwards compatible change](#TODO). In preference order:

1. use `PUT` with complete objects to update a resource as long as feasible (i.e. do not use `PATCH` at all). 
2. use `PATCH` with partial objects to only update parts of a resource, whenever possible. (This is basically [JSON Merge Patch](https://tools.ietf.org/html/rfc7396), a specialized media type `application/merge-patch+json` that is a partial resource representation.)
3. use `PATCH` with [JSON Patch](https://tools.ietf.org/html/rfc6902), a specialized media type `application/json-patch+json` that includes instructions on how to change the resource.
4. use `POST` (with a proper description of what is happening) instead of `PATCH`, if the request does not modify the resource in a way defined by the semantics of the media type.

In practice [JSON Merge Patch](https://tools.ietf.org/html/rfc7396) quickly turns out to be too limited, especially when trying to update single objects in large collections (as part of the resource). In this cases [JSON Patch](https://tools.ietf.org/html/rfc6902) can shown its full power while still showing readable patch requests (see also [JSON patch vs. merge](http://erosb.github.io/post/json-patch-vs-merge-patch)).

**Note**: Patching the same resource twice is not required to be [idempotent](#TODO) (check [MUST fulfill common method properties](#TODO)) and may result in a changing result. However, you [SHOULD consider to design POST and PATCH idempotent](#TODO) to prevent this.

**Hint**: To prevent unnoticed concurrent updates when using PATCH you [MAY consider to support ETag together with If-Match/If-None-Match header](#TODO) to allow the server to react on stricter demands that expose conflicts and prevent lost updates. See [Optimistic locking in RESTful APIs](#TODO) and [SHOULD consider to design POST and PATCH idempotent](#TODO) for details and options.

#### DELETE

`DELETE` requests are used to **delete** resources. The semantic is best described as "please delete the resource identified by the URL".

- `DELETE` requests are usually applied to single resources, not on collection resources, as this would imply deleting the entire collection.
- `DELETE` request can be applied to multiple resources at once using query parameters on the collection resource (see [`DELETE` with query parameters](#TODO)).
- successful `DELETE` requests will usually generate `200` (if the deleted resource is returned) or `204` (if no content is returned).
- failed `DELETE` requests will usually generate `404` (if the resource cannot be found) or `410` (if the resource was already deleted before).

**Important**: After deleting a resource with `DELETE`, a `GET` request on the resource is expected to either return `404` (not found) or `410` (gone) depending on how the resource is represented after deletion. Under no circumstances the resource must be accessible after this operation on its endpoint.

#### DELETE with query parameters

`DELETE` request can have query parameters. Query parameters should be used as filter parameters on a resource and not for passing context information to control the operation behavior.

```http request
DELETE /resources?param1=value1&param2=value2...&paramN=valueN
```

**Note**: When providing `DELETE` with query parameters, API designers must carefully document the behavior in case of (partial) failures to manage client expectations properly.

The response status code of `DELETE` with query parameters requests should be similar to usual `DELETE` requests. In addition, it may return the status code `207` using a payload describing the operation results (see [MUST use code 207 for batch or bulk requests](#TODO) for details).

#### DELETE with body

In rare cases `DELETE` may require additional information, that cannot be classified as filter parameters and thus should be transported via payload, to perform the operation. Since [RFC-7231](https://tools.ietf.org/html/rfc7231#section-4.3.5) states, that `DELETE` has an undefined semantic for payloads, we recommend to utilize POST in this cases similar to `GET with body`.

In this case the endpoint must be documented with the hint `DELETE` with body to transport the `DELETE` semantic of this call. The response status code of `DELETE with body` requests should be similar to usual `DELETE` requests.

```http request
POST /resources/{resource-id}

{
  "prop1": "value1",
  ...
  "propN": "valueN",
}
```

#### HEAD

`HEAD` requests are used to **retrieve** the header information of single resources and resource collections.

- `HEAD` has exactly the same semantics as `GET`, but returns headers only, no body.

**Hint**: `HEAD` is particular useful to efficiently lookup whether large resources or collection resources have been updated in conjunction with the `ETag`-header.

#### OPTIONS

`OPTIONS` requests are used to **inspect** the available operations (HTTP methods) of a given endpoint.

- `OPTIONS` responses usually either return a comma separated list of methods in the `Allow` header or as a structured list of link templates

**Note**: OPTIONS is rarely implemented, though it could be used to self-describe the full functionality of a resource.


### MUST fulfill common method properties <a id="1302"></a>

Request methods in RESTful services can be...

- [safe](https://tools.ietf.org/html/rfc7231#section-4.2.1) - the operation semantic is defined to be read-only, meaning it must not have intended side effects, i.e. changes, to the server state.
- [idempotent](https://tools.ietf.org/html/rfc7231#section-4.2.2) - the operation has the same intended effect on the server state, independently whether it is executed once or multiple times. **Note**: this does not require that the operation is returning the same response or status code.
- [cacheable](https://tools.ietf.org/html/rfc7231#section-4.2.3) - to indicate that responses are allowed to be stored for future reuse. In general, requests to safe methods are cachable, if it does not require a current or authoritative response from the server.

**Note**: The above definitions, of _intended (side) effect_ allows the server to provide additional state changing behavior as logging, accounting, pre- fetching, etc. However, these actual effects and state changes, must not be intended by the operation so that it can be held accountable.

Method implementations must fulfill the following basic properties according to [RFC 7231](https://tools.ietf.org/html/rfc7231):

| Method  | Safe  | Idempotent                                                    | Cacheable                                                                              |
|---------|-------|---------------------------------------------------------------|----------------------------------------------------------------------------------------|
| GET     | ✔ Yes | ✔ Yes                                                         | ✔ Yes                                                                                  |
| HEAD    | ✔ Yes | ✔ Yes                                                         | ✔ Yes                                                                                  |
| POST    | ✗ No  | ⚠️ No, but [SHOULD consider to design POST and PATCH idempotent](#TODO) | ⚠️ May, but only if specific POST endpoint is safe. Hint: not supported by most caches. |
| PUT     | ✗ No  | ✔ Yes                                                         | ✗ No                                                                                   |
| PATCH   | ✗ No  | ⚠️ No, but [SHOULD consider to design POST and PATCH idempotent](#TODO) | ✗ No                                                                                   |
| DELETE  | ✗ No  | ✔ Yes                                                         | ✗ No                                                                                   |
| OPTIONS | ✔ Yes | ✔ Yes                                                         | ✗ No                                                                                   |
| TRACE   | ✔ Yes | ✔ Yes                                                         | ✗ No                                                                                   |

**Note**: [MUST document cachable GET, HEAD, and POST endpoints](#TODO).

### SHOULD consider to design POST and PATCH idempotent <a id="1303"></a>

In many cases it is helpful or even necessary to design `POST` and `PATCH` [idempotent](#TODO) for clients to expose conflicts and prevent resource duplicate (a.k.a. zombie resources) or lost updates, e.g. if same resources may be created or changed in parallel or multiple times. To design an [idempotent](#TODO) API endpoint owners should consider to apply one of the following three patterns.

- A resource specific **conditional key** provided via `If-Match` header in the request. The key is in general a meta information of the resource, e.g. a hash or version number, often stored with it. It allows to detect concurrent creations and updates to ensure [idempotent](#TODO) behavior (see [MAY consider to support ETag together with If-Match/If-None-Match header](#TODO)).
- A resource specific **secondary key** provided as resource property in the request body. The secondary key is stored permanently in the resource. It allows to ensure [idempotent](#TODO) behavior by looking up the unique secondary key in case of multiple independent resource creations from different clients (see [SHOULD use secondary key for idempotent POST design](#TODO)).
- A client specific **idempotency key** provided via `Idempotency-Key` header in the request. The key is not part of the resource but stored temporarily pointing to the original response to ensure [idempotent](#TODO) behavior when retrying a request (see [MAY consider to support Idempotency-Key header](#TODO)).

**Note**: While **conditional key** and **secondary key** are focused on handling concurrent requests, the **idempotency key** is focused on providing the exact same responses, which is even a stronger requirement than the [idempotency defined above](#TODO). It can be combined with the two other patterns.

To decide, which pattern is suitable for your use case, please consult the following table showing the major properties of each pattern:

|                                       | Conditional Key | Secondary Key | Idempotency Key |
|:-------------------------------------:|:---------------:|:-------------:|:---------------:|
| Applicable with                       | PATCH           | POST          | POST/PATCH      |
| HTTP Standard                         | ✔ Yes           | ✗ No          | ✗ No            |
| Prevents duplicate (zombie) resources | ✔ Yes           | ✔ Yes         | ✗ No            |
| Prevents concurrent lost updates      | ✔ Yes           | ✗ No          | ✗ No            |
| Supports safe retries                 | ✔ Yes           | ✔ Yes         | ✔ Yes           |
| Supports exact same response          | ✗ No            | ✗ No          | ✔ Yes           |
| Can be inspected (by intermediaries)  | ✔ Yes           | ✗ No          | ✔ Yes           |
| Usable without previous GET           | ✗ No            | ✔ Yes         | ✔ Yes           |

**Note**: The patterns applicable to `PATCH` can be applied in the same way to `PUT` and `DELETE` providing the same properties.

If you mainly aim to support safe retries, we suggest to apply [conditional key](#TODO) and [secondary key](@TODO) pattern before the [Idempotency Key](#TODO) pattern.

### SHOULD use secondary key for idempotent POST design <a id="1304"></a>

The most important pattern to design `POST` [idempotent](#TODO) for creation is to introduce a resource specific **secondary key** provided in the request body, to eliminate the problem of duplicate (a.k.a zombie) resources.

The secondary key is stored permanently in the resource as _alternate key_ or _combined key_ (if consisting of multiple properties) guarded by a uniqueness constraint enforced server-side, that is visible when reading the resource. The best and often naturally existing candidate is a _unique foreign key_, that points to another resource having _one-on-one_ relationship with the newly created resource, e.g. a parent process identifier.

A good example here for a secondary key is the shopping cart ID in an order resource.

**Note**: When using the secondary key pattern without `Idempotency-Key` all subsequent retries should fail with status code `409` (conflict). We suggest to avoid `200` here unless you make sure, that the delivered resource is the original one implementing a well defined behavior. Using `204` without content would be a similar well defined option.

### MUST define collection format of header and query parameters <a id="1305"></a>

Header and query parameters allow to provide a collection of values, either by providing a comma-separated list of values or by repeating the parameter multiple times with different values as follows:

| Parameter Type | Comma-separated Values | Multiple Parameters            | Standard               |
|----------------|------------------------|--------------------------------|------------------------|
| Header         | Header: value1,value2  | Header: value1, Header: value2 | [RFC 7230 Section 3.2.2](https://tools.ietf.org/html/rfc7230#section-3.2.2) |
| Query          | ?param=value1,value2   | ?param=value1&param=value2     | [RFC 6570 Section 3.2.8](https://tools.ietf.org/html/rfc6570#section-3.2.8) |

As Open API does not support both schemas at once, an API specification must explicitly define the collection format to guide consumers as follows:


| Parameter Type | Comma-separated Values        | Multiple Parameters                      |
|----------------|-------------------------------|------------------------------------------|
| Header         | style: simple, explode: false | not allowed (see [RFC 7230 Section 3.2.2](https://tools.ietf.org/html/rfc7230#section-3.2.2)) |
| Query          | style: form, explode: false   | style: form, explode: true               |

When choosing the collection format, take into account the tool support, the escaping of special characters and the maximal URL length.

### SHOULD design simple query languages using query parameters <a id="1306"></a>

We prefer the use of query parameters to describe resource-specific query languages for the majority of APIs because it’s native to HTTP, easy to extend and has excellent implementation support in HTTP clients and web frameworks.

Query parameters should have the following aspects specified:

- Reference to corresponding property, if any
- Value range, e.g. inclusive vs. exclusive
- Comparison semantics (equals, less than, greater than, etc)
- Implications when combined with other queries, e.g. _and_ vs. _or_

How query parameters are named and used is up to individual API designers. The following examples should serve as ideas:

- `distributor=BNL`, to query for elements based on property equality
- `age=55`, to query for elements based on logical properties
  - Assuming that elements don’t actually have an `age` but rather a `birthday`
- `max_length=5`, based on upper and lower bounds (`min` and `max`)
- `shorter_than=5`, using terminology specific e.g. to `length`
- `created_before=2019-07-17` or `not_modified_since=2019-07-17`
  -  Using terminology specific e.g. to time: `before`, `after`, `since` and `until`

We don’t advocate for or against certain names because in the end APIs should be free to choose the terminology that fits their domain the best.

### SHOULD design complex query languages using JSON <a id="1307"></a>

Minimalistic query languages based on [query parameters](#TODO) are suitable for simple use cases with a small set of available filters that are combined in one way and one way only (e.g. _and_ semantics). Simple query languages are generally preferred over complex ones.

Some APIs will have a need for sophisticated and more complex query languages. Dominant examples are APIs around search (incl. faceting) and product catalogs.

Aspects that set those APIs apart from the rest include but are not limited to:

- Unusual high number of available filters
- Dynamic filters, due to a dynamic and extensible resource model
- Free choice of operators, e.g. `and`, `or` and `not`

APIs that qualify for a specific, complex query language are encouraged to use nested JSON data structures and define them using Open API directly. The provides the following benefits:

- Data structures are easy to use for clients
  - No special library support necessary
  - No need for string concatenation or manual escaping
- Data structures are easy to use for servers
  - No special tokenizers needed
  - Semantics are attached to data structures rather than text tokens
- Consistent with other HTTP methods
- API is defined in Open API completely
  - No external documents or grammars needed
  - Existing means are familiar to everyone

[JSON-specific rules](#TODO) and most certainly needs to make use of the [GET-with-body](#TODO) pattern.

#### Example

The following JSON document should serve as an idea how a structured query might look like.

```json
{
  "and": {
    "name": {
      "match": "Alice"
    },
    "age": {
      "or": {
        "range": {
          ">": 25,
          "<=": 50
        },
        "=": 65
      }
    }
  }
}
```

### MUST document implicit filtering <a id="1308"></a>

Sometimes certain collection resources or queries will not list all the possible elements they have, but only those for which the current client is authorized to access.

Implicit filtering could be done on:

- the collection of resources being return on a parent `GET` request
- the fields returned for the resource’s detail

In such cases, the implicit filtering must be in the API specification (in its description).

Consider [caching considerations](TODO) when implicitly filtering.

#### Example

If an employee of the company _Foo_ accesses one of our business-to-business service and performs a `GET /business-partners`, it must, for legal reasons, not display any other business partner that is not owned or contractually managed by her/his company. It should never see that we are doing business also with company _Bar_.

Response as seen from a consumer working at `FOO`:

```json
{
    "items": [
        { "name": "Foo Performance" },
        { "name": "Foo Documents" },
        { "name": "Foo Signature" }
    ]
}
```

Response as seen from a consumer working at BAR:

```json
{
    "items": [
        { "name": "Bar Options" },
        { "name": "Bar Products" }
    ]
}
```

The API Specification should then specify something like this:

```yaml
paths:
  /business-partner:
    get:
      description: >-
        Get the list of registered business partner.
        Only the business partners to which you have access to are returned.

```

## 14. HTTP status codes and errors <a id="1400"></a>

### MUST specify success and error responses <a id="1401"></a>

APIs should define the functional, business view and abstract from implementation aspects. Success and error responses are a vital part to define how an API is used correctly.

Therefore, you must define **all** success and service specific error responses in your API specification. Both are part of the interface definition and provide important information for service clients to handle standard as well as exceptional situations.

**Hint**: In most cases it is not useful to document all technical errors, especially if they are not under control of the service provider. Thus, unless a response code conveys application-specific functional semantics or is used in a none standard way that requires additional explanation, multiple error response specifications can be combined using the following pattern (see also [MUST only use durable and immutable remote references](#TODO)):

```yaml
responses:
  default:
    description: error occurred - see status code and problem object for more information.
    content:
      "application/json":
        schema:
          $ref: '#/definitions/Error'
```

API designers should also think about a troubleshooting board or WIKI as part of the associated online API documentation. It provides information and handling guidance on application-specific errors and is referenced via links from the API specification. This can reduce service support tasks and contribute to service client and provider performance.

### MUST use standard HTTP status codes <a id="1402"></a>

You must only use standardized HTTP status codes consistently with their intended semantics. You must not invent new HTTP status codes.

RFC standards define ~60 different HTTP status codes with specific semantics (mainly [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6) and [RFC 6585](https://tools.ietf.org/html/rfc6585) — and there are upcoming new ones, e.g. draft [legally-restricted-status](https://tools.ietf.org/html/draft-tbray-http-legally-restricted-status-05). See overview on all error codes on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) or via [https://httpstatuses.com](https://httpstatuses.com/)) also inculding 'unofficial codes', e.g. used by popular web servers like Nginx.

Below we list the most commonly used and best understood HTTP status codes, consistent with their semantic in the RFCs. APIs should only use these to prevent misconceptions that arise from less commonly used HTTP status codes.

**Important**: As long as your HTTP status code usage is well covered by the semantic defined here, you should not describe it to avoid an overload with common sense information and the risk of inconsistent definitions. Only if the HTTP status code is not in the list below or its usage requires additional information aside the well defined semantic, the API specification must provide a clear description of the HTTP status code in the response.

#### Success codes

| Code | Meaning                                                                                                                                                                                                                                        | Methods                  |
|------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| `200`  | OK - this is the standard success response                                                                                                                                                                                                     | <all>                    |
| `201`  | Created - Returned on successful entity creation. You are free to return either an empty response or the created resource in conjunction with the Location header. (More details found in the Common headers.) Always set the Location header. | `POST`, `PUT`                |
| `202`  | Accepted - The request was successful and will be processed asynchronously.                                                                                                                                                                    | `POST`, `PUT`, `PATCH`, `DELETE` |
| `204`  | No content - There is no response body.                                                                                                                                                                                                        | `PUT`, `PATCH`, `DELETE`       |
| `207`  | Multi-Status - The response body contains multiple status informations for different parts of a batch/bulk request (see [MUST use code 207 for batch or bulk requests](#TODO)).                                                                         | `POST`, (`DELETE`)           |


#### Redirection codes

| Code | Meaning                                                                                                                                                                                                                                                                                             | Methods                  |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| `301`  | Moved Permanently - This and all future requests should be directed to the given URI.                                                                                                                                                                                                               | <all>                    |
| `303`  | See Other - The response to the request can be found under another URI using a `GET` method.                                                                                                                                                                                                          | `POST`, `PUT`, `PATCH`, `DELETE` |
| `304`  | Not Modified - indicates that a conditional GET or HEAD request would have resulted in 200 response if it were not for the fact that the condition evaluated to false, i.e. resource has not been modified since the date or version passed via request headers `If-Modified-Since` or `If-None-Match`. | `GET`, `HEAD`                |

#### Client side codes

| Code | Meaning                                                                                                                                                                                                          | Methods                  |
|------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| `400`  | Bad request - generic / unknown error. Should also be delivered in case of input payload fails business logic validation.                                                                                        | <all>                    |
| `401`  | Unauthorized - the users must log in (this often means "Unauthenticated").                                                                                                                                       | <all>                    |
| `403`  | Forbidden - the user is not authorized to use this resource.                                                                                                                                                     | <all>                    |
| `404`  | Not found - the resource is not found.                                                                                                                                                                           | <all>                    |
| `405`  | Method Not Allowed - the method is not supported, see `OPTIONS`.                                                                                                                                                   | <all>                    |
| `406`  | Not Acceptable - resource can only generate content not acceptable according to the `Accept` headers sent in the request.                                                                                          | <all>                    |
| `408`  | Request timeout - the server times out waiting for the resource.                                                                                                                                                 | <all>                    |
| `409`  | Conflict - request cannot be completed due to conflict, e.g. when two clients try to create the same resource or if there are concurrent, conflicting updates.                                                   | `POST`, `PUT`, `PATCH`, `DELETE` |
| `410`  | Gone - resource does not exist any longer, e.g. when accessing a resource that has intentionally been deleted.                                                                                                   | <all>                    |
| `412`  | Precondition Failed - returned for conditional requests, e.g. `If-Match` if the condition failed. Used for optimistic locking.                                                                                     | `PUT`, `PATCH`, `DELETE`       |
| `415`  | Unsupported Media Type - e.g. clients sends request body without content type.                                                                                                                                   | `POST`, `PUT`, `PATCH`, `DELETE` |
| `423`  | Locked - Pessimistic locking, e.g. processing states.                                                                                                                                                            | `PUT`, `PATCH`, `DELETE`       |
| `428`  | Precondition Required - server requires the request to be conditional, e.g. to make sure that the "lost update problem" is avoided (see [MAY consider to support Prefer header to handle processing preferences](#TODO)). | <all>                    |
| `429`  | Too many requests - the client does not consider rate limiting and sent too many requests (see [MUST use code 429 with headers for rate limits](#TODO)).                                                                  | <all>                    |


#### Server side codes

| Code | Meaning                                                                                                                                                                                                                                                                        | Methods |
|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| `500`  | Internal Server Error - a generic error indication for an unexpected server execution problem (here, client retry may be sensible)                                                                                                                                             | <all>   |
| `501`  | Not Implemented - server cannot fulfill the request (usually implies future availability, e.g. new feature).                                                                                                                                                                   | <all>   |
| `503`  | Service Unavailable - service is (temporarily) not available (e.g. if a required component or downstream service is not available) - client retry may be sensible. If possible, the service should indicate how long the client should wait by setting the `Retry-After` header. | <all>   |

### MUST use most specific HTTP status codes <a id="1403"></a>

You must use the most specific HTTP status code when returning information about your request processing status or error situations.

### MUST use code 207 for batch or bulk requests <a id="1403"></a>

Some APIs are required to provide either _batch_ or _bulk_ requests using `POST` for performance reasons, i.e. for communication and processing efficiency. In this case services may be in need to signal multiple response codes for each part of an batch or bulk request. As HTTP does not provide proper guidance for handling batch/bulk requests and responses, we herewith define the following approach:


- A batch or bulk request **always** responds with HTTP status code `207` unless a non-item-specific failure occurs.
- A batch or bulk request MAY return `4xx/5xx` status codes, if the failure is non-item-specific and cannot be restricted to individual items of the batch or bulk request, e.g. in case of overload situations or general service failures.
- A batch or bulk response with status code `207` **always** returns as payload a multi-status response containing item specific status and/or monitoring information for each part of the batch or bulk request.

**Note**: These rules apply _even in the case_ that processing of all individual parts fail or each part is executed _asynchronously_!

The rules are intended to allow clients to act on batch and bulk responses in a consistent way by inspecting the individual results. We explicitly reject the option to apply `200` for a completely successful batch as proposed in Nakadi’s `POST /event-types/{name}/events` as short cut without inspecting the result, as we want to avoid risks and expect clients to handle partial batch failures anyway.

The bulk or batch response may look as follows:

```yaml
BatchOrBulkResponse:
  description: batch response object.
  type: object
  properties:
    items:
      type: array
      items:
        type: object
        properties:
          id:
            description: Identifier of batch or bulk request item.
            type: string
          status:
            description: >
              Response status value. A number or extensible enum describing
              the execution status of the batch or bulk request items.
            type: string
            enum: [...]
          description:
            description: >
              Human readable status description and containing additional
              context information about failures etc.
            type: string
        required: [id, status]
```

**Note**: while a batch defines a collection of requests triggering independent processes, a _bulk_ defines a collection of independent resources created or updated together in one request. With respect to response processing this distinction normally does not matter.

### MUST use code 429 with headers for rate limits <a id="1404"></a>

APIs that wish to manage the request rate of clients must use the `429` (Too Many Requests) response code, if the client exceeded the request rate (see [RFC 6585](https://tools.ietf.org/html/rfc6585)). Such responses must also contain header information providing further details to the client. There are two approaches a service can take for header information:

- Return a `Retry-After` header indicating how long the client ought to wait before making a follow-up request. The Retry-After header can contain a HTTP date value to retry after or the number of seconds to delay. Either is acceptable but APIs should prefer to use a delay in seconds.
- Return a trio of `X-RateLimit` headers. These headers (described below) allow a server to express a service level in the form of a number of allowing requests within a given window of time and when the window is reset.

The `X-RateLimit` headers are:

- `X-RateLimit-Limit`: The maximum number of requests that the client is allowed to make in this window.
- `X-RateLimit-Remaining`: The number of requests allowed in the current window.
- `X-RateLimit-Reset`: The relative time in seconds when the rate limit window will be reset. Beware that this is different to Github and Twitter’s usage of a header with the same name which is using UTC epoch seconds instead.

The reason to allow both approaches is that APIs can have different needs. `Retry-After` is often sufficient for general load handling and request throttling scenarios and notably, does not strictly require the concept of a calling entity such as a tenant or named account. In turn this allows resource owners to minimise the amount of state they have to carry with respect to client requests. The `X-RateLimit` headers are suitable for scenarios where clients are associated with pre-existing account or tenancy structures. `X-RateLimit` headers are generally returned on every request and not just on a `429`, which implies the service implementing the API is carrying sufficient state to track the number of requests made within a given window for each named entity.

### MUST use problem JSON <a id="1405"></a>

[RFC 7807](https://tools.ietf.org/html/rfc7807) defines a Problem JSON object and the media type application/problem+json. Operations should return it (together with a suitable status code) when any problem occurred during processing and you can give more details than the status code itself can supply, whether it be caused by the client or the server (i.e. both for `4xx` or `5xx` error codes).

You may define custom problem types as extensions of the Problem JSON object if your API needs to return specific, additional and detailed error information.

Problem `type` and `instance` identifiers in our APIs are not meant to be resolved. [RFC 7807](https://tools.ietf.org/html/rfc7807) encourages that custom problem types are URI references that point to human-readable documentation, but we deliberately decided against that, as all important parts of the API must be documented using [OpenAPI](https://swagger.io/specification/) anyway. In addition, URLs tend to be fragile and not very stable over longer periods because of organizational and documentation changes and descriptions might easily get out of sync.

In order to stay compatible with [RFC 7807](https://tools.ietf.org/html/rfc7807) we proposed to use [relative URI references](https://tools.ietf.org/html/rfc3986#section-4.1) usually defined by absolute-path `[ '?' query ] [ '#' fragment ]` as s`implified identifiers in type and instance fields:

- `/problems/out-of-stock`
- `/problems/insufficient-funds`
- `/problems/user-deactivated`
- `/problems/connection-error#read-timeout`

**Note**: The use of absolute URIs is not forbidden but strongly discouraged.

### MUST not expose stack traces <a id="1406"></a>

Stack traces contain implementation details that are not part of an API, and on which clients should never rely. Moreover, stack traces can leak sensitive information that partners and third parties are not allowed to receive and may disclose insights about vulnerabilities to attackers.

## 15. Performance <a id="1500"></a>

### SHOULD reduce bandwidth needs and improve responsiveness <a id="1501"></a>

APIs should support techniques for reducing bandwidth based on client needs. This holds for APIs that (might) have high payloads and/or are used in high-traffic scenarios like the public Internet and telecommunication networks. Typical examples are APIs used by mobile web app clients with (often) less bandwidth connectivity.

Common techniques include:

- compression of request and response bodies (see [SHOULD use gzip compression](#1502))
- querying field filters to retrieve a subset of resource attributes (see [SHOULD support partial responses via filtering](#1503) below)
- `ETag` and` If-Match`/`If-None-Match` headers to avoid re-fetching of unchanged resources (see [MAY consider to support ETag together with If-Match/If-None-Match header](#TODO))
- Prefer header with `return=minimal` or `respond-async` to anticipate reduced processing requirements of clients (see [MAY consider to support Prefer header to handle processing preferences](#TODO))
- [Pagination](#TODO) for incremental access of larger collections of data items
- caching of master data items, i.e. resources that change rarely or not at all after creation (see [MUST document cachable GET, HEAD, and POST endpoints](#TODO)).

Each of these items is described in greater detail below.

### SHOULD use gzip compression <a id="1502"></a>

Compress the payload of your API’s responses with gzip, unless there’s a good reason not to — for example, you are serving so many requests that the time to compress becomes a bottleneck. This helps to transport data faster over the network (fewer bytes) and makes frontends respond faster.

Though gzip compression might be the default choice for server payload, the server should also support payload without compression and its client control via `Accept-Encoding` request header - see also [RFC 7231 Section 5.3.4](#TODO). The server should indicate used gzip compression via the `Content-Encoding` header.

### SHOULD support partial responses via filtering <a id="1503"></a>

Depending on your use case and payload size, you can significantly reduce network bandwidth need by supporting filtering of returned entity fields. Here, the client can explicitly determine the subset of fields he wants to receive via the fields query parameter. (It is analogue to [GraphQL](https://graphql.org/learn/queries/#fields) `fields` and simple queries, and also applied, for instance, for [Google Cloud API’s partial responses](https://cloud.google.com/storage/docs/json_api/v1/how-tos/performance#partial-response).)

#### Unfiltered

```http request
GET http://api.example.org/users/123 HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "cddd5e44-dae0-11e5-8c01-63ed66ab2da5",
  "name": "John Doe",
  "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
  "birthday": "1984-09-13",
  "friends": [ {
    "id": "1fb43648-dae1-11e5-aa01-1fbc3abb1cd0",
    "name": "Jane Doe",
    "address": "1600 Pennsylvania Avenue Northwest, Washington, DC, United States",
    "birthday": "1988-04-07"
  } ]
}
```

#### Filtered

```http request
GET http://api.example.org/users/123?fields=(name,friends(name)) HTTP/1.1

HTTP/1.1 200 OK
Content-Type: application/json

{
  "name": "John Doe",
  "friends": [ {
    "name": "Jane Doe"
  } ]
}
```

The `fields` query parameter determines the fields returned with the response payload object. For instance, (`name`) returns `users` root object with only the `name` field, and (`name,friends(name)`) returns the `name` and the `nested` friends object with only its `name` field.

**Note**: Following the [principle of least astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment), you should not define the fields query parameter using a default value, as the result is counter-intuitive and very likely not anticipated by the consumer.

### SHOULD allow optional embedding of sub-resources <a id="1504"></a>

Embedding related resources (also know as Resource expansion) is a great way to reduce the number of requests. In cases where clients know upfront that they need some related resources they can instruct the server to prefetch that data eagerly. Whether this is optimized on the server, e.g. a database join, or done in a generic way, e.g. an HTTP proxy that transparently embeds resources, is up to the implementation.

Embedding a sub-resource can possibly look like this where an order resource has its order items as sub-resource (`/order/{orderId}/items`):

```http request
GET /order/123?embed=(items) HTTP/1.1

{
  "id": "123",
  "_embedded": {
    "items": [
      {
        "position": 1,
        "sku": "1234-ABCD-7890",
        "price": {
          "amount": 71.99,
          "currency": "EUR"
        }
      }
    ]
  }
}

```

### MUST document cachable GET, HEAD, and POST endpoints <a id="1505"></a>

Caching has to take many aspects into account, e.g. general cacheability of response information, resource update and invalidation rules, existence of multiple consumer instances, etc. As a consequence, caching is in best case complex, e.g. with respect to consistency, in worst case inefficient.

As a consequence, client side as well as transparent web caching should be avoided, unless the service supports and requires it to protect itself, e.g. in case of a heavily used and therefore rate limited master data service, i.e. data items that rarely or not at all change after creation.

As default, API providers and consumers should always set the `Cache-Control` header set to `Cache-Control: no-store` and assume the same setting, if no `Cache-Control` header is provided.

**Note**: There is no need to document this default setting. However, please make sure that your framework is attaching this header value by default, or ensure this manually, e.g. using the best practice of Spring Security as shown below. Any setup deviating from this default must be sufficiently documented.

```http request
Cache-Control: no-cache, no-store, must-revalidate, max-age=0
```

If your service really requires to support caching, please observe the following rules:

- Document all cacheable `GET`, `HEAD`, and `POST` endpoints by declaring the support of `Cache-Control`, `Vary`, and `ETag` headers in response. **Note**: you must not define the `Expires` header to prevent redundant and ambiguous definition of cache lifetime. A sensible default documentation of these headers is given below.
- Take care to specify the ability to support caching by defining the right caching boundaries, i.e. time-to-live and cache constraints, by providing sensible values for `Cache-Control` and `Vary` in your service. We will explain best practices below.
- Provide efficient methods to warm up and update caches, e.g. as follows:
  - In general, you should support `ETag` together with `If-Match`/ `If-None-Match` header on all cacheable endpoints.
  - For larger data items support `HEAD` requests or more efficient `GET` requests with `If-None-Match` header to check for updates.
  - For small data sets provide full collection `GET` requests supporting `ETag`, as well as `HEAD` requests or `GET` requests with `If-None-Match` to check for updates.
  - For medium sized data sets provide full collection `GET` requests supporting `ETag` together with [Pagination](#TODO) and `<entity-tag>` filtering `GET` requests for limiting the response to changes since the provided `<entity-tag>`. Note: this is not supported by generic client and proxy caches on HTTP layer

**Hint**: For proper cache support, you must return `304` without content on a failed `HEAD` or `GET` request with `If-None-Match: <entity-tag>` instead of `412`.

```yaml
components:
  headers:
  - Cache-Control:
      description: |
        The RFC 7234 Cache-Control header field is providing directives to
        control how proxies and clients are allowed to cache responses results
        for performance. Clients and proxies are free to not support caching of
        results, however if they do, they must obey all directives mentioned in
        [RFC-7234 Section 5.2.2](https://tools.ietf.org/html/rfc7234) to the
        word.

        In case of caching, the directive provides the scope of the cache
        entry, i.e. only for the original user (private) or shared between all
        users (public), the lifetime of the cache entry in seconds (max-age),
        and the strategy how to handle a stale cache entry (must-revalidate).
        Please note, that the lifetime and validation directives for shared
        caches are different (s-maxage, proxy-revalidate).

      type: string
      required: false
      example: "private, must-revalidate, max-age=300"

  - Vary:
      description: |
        The RFC 7231 Vary header field in a response defines which parts of
        a request message, aside the target URL and HTTP method, might have
        influenced the response. A client or proxy cache must respect this
        information, to ensure that it delivers the correct cache entry (see
        [RFC-7231 Section
        7.1.4](https://tools.ietf.org/html/rfc7231#section-7.1.4)).

      type: string
      required: false
      example: "accept-encoding, accept-language"
```

**Hint**: For `ETag` source see [MAY consider to support ETag together with If-Match/If-None-Match header](#TODO).

The default setting for `Cache-Control` should contain the `must-revalidate` directive to ensure, that the client does not use stale cache entries. Last, the `max-age` directive should be set to a value between a few seconds (`max-age=60`) and a few hours (`max-age=86400`) depending on the change rate of your master data and your requirements to keep clients consistent.

```http request
Cache-Control: private, must-revalidate, max-age=300
```

The default setting for `Vary` is harder to determine correctly. It highly depends on the API endpoint, e.g. whether it supports compression, accepts different media types, or requires other request specific headers. To support correct caching you have to carefully choose the value. However, a good first default may be:

```http request
Vary: accept, accept-encoding
```

Anyhow, this is only relevant, if you encourage clients to install generic HTTP layer client and proxy caches.

**Note**: generic client and proxy caching on HTTP level is hard to configure. Therefore, we strongly recommend to attach the (possibly distributed) cache directly to the service (or gateway) layer of your application. This relieves from interpreting the `Vary` header and greatly simplifies interpreting the `Cache-Control` and `ETag` headers. Moreover, is highly efficient with respect to caching performance and overhead, and allows to support more advanced cache update and warm up patterns.

Anyhow, please carefully read [RFC 7234](https://tools.ietf.org/html/rfc7234) before adding any client or proxy cache.

## 16. Pagination <a id="1600"></a>

### MUST support pagination <a id="1601"></a>

Access to lists of data items must support pagination to protect the service against overload as well as for best client side iteration and batch processing experience. This holds true for all lists that are (potentially) larger than just a few hundred entries.

There are two well known page iteration techniques:

- [Offset/Limit-based pagination](https://developer.infoconnect.com/paging-results): numeric offset identifies the first page entry
- [Cursor/Limit-based](https://dev.twitter.com/overview/api/cursoring) — aka key-based — pagination: a unique key element identifies the first page entry (see also [Facebook’s guide](https://developers.facebook.com/docs/graph-api/using-graph-api/v2.4#paging))

The technical conception of pagination should also consider user experience related issues. As mentioned in this [article](https://www.smashingmagazine.com/2016/03/pagination-infinite-scrolling-load-more-buttons), jumping to a specific page is far less used than navigation via next/prev page links (See [SHOULD use pagination links where applicable](#TODO)). This favours cursor-based over offset-based pagination.

**Note**: To provide a consistent look and feel of pagination patterns, you must stick to the common query parameter names defined in [MUST stick to conventional query parameters](#TODO).


### SHOULD prefer cursor-based pagination, avoid offset-based pagination <a id="1602"></a>

Cursor-based pagination is usually better and more efficient when compared to offset-based pagination. Especially when it comes to high-data volumes and/or storage in NoSQL databases.

Before choosing cursor-based pagination, consider the following trade-offs:

- Usability/framework support:
  - Offset-based pagination is more widely known than cursor-based pagination, so it has more framework support and is easier to use for API clients
- Use case - jump to a certain page:
  - If jumping to a particular page in a range (e.g., 51 of 100) is really a required use case, cursor-based navigation is not feasible.
- Data changes may lead to anomalies in result pages:
  - Offset-based pagination may create duplicates or lead to missing entries if rows are inserted or deleted between two subsequent paging requests.
  - If implemented incorrectly, cursor-based pagination may fail when the cursor entry has been deleted before fetching the pages.
- Performance considerations - efficient server-side processing using offset-based pagination is hardly feasible for:
  - Very big data sets, especially if they cannot reside in the main memory of the database.
  - Sharded or NoSQL databases.
- Cursor-based navigation may not work if you need the total count of results.

The `cursor` used for pagination is an opaque pointer to a page, that must never be **inspected** or **constructed** by clients. It usually encodes (encrypts) the page position, i.e. the identifier of the first or last page element, the pagination direction, and the applied query filters - or a hash over these - to safely recreate the collection. The `cursor` may be defined as follows:

```yaml
Cursor:
  type: object
  properties:
    position:
      description: >
        Object containing the identifier(s) pointing to the entity that is
        defining the collection resource page - normally the position is
        represented by the first or the last page element.
      type: object
      properties: ...

    direction:
      description: >
        The pagination direction that is defining which elements to choose
        from the collection resource starting from the page position.
      type: string
      enum: [ ASC, DESC ]

    query:
      description: >
        Object containing the query filters applied to create the collection
        resource that is represented by this cursor.
      type: object
      properties: ...

    query_hash:
      description: >
        Stable hash calculated over all query filters applied to create the
        collection resource that is represented by this cursor.
      type: string

  required:
    - position
    - direction
```

The page information for cursor-based pagination should consist of a `cursor` set, that besides `next` may provide support for `prev`, `first`, `last`, and `self` as follows (see also [Link relation fields](#TODO)):

```json
{
  "cursors": {
    "self": "...",
    "first": "...",
    "prev": "...",
    "next": "...",
    "last": "..."
  },
  "items": [...]
}
```

**Note**: The support of the cursor set may be dropped in favor of [SHOULD use pagination links where applicable](#TODO).


### SHOULD use pagination links where applicable <a id="1603"></a>

To simplify client design, APIs should support [simplified hypertext controls](#TODO) for pagination over collections whenever applicable. Beside `next` this may comprise the support for `prev`, `first`, `last`, and `self` as [link relations](#TODO) (see also [Link relation fields](#TODO) for details).

The page content is transported via `items`, while the `query` object may contain the query filters applied to the collection resource as follows:

```json
{
  "self": "http://my-service.bnlapis.com/resources?cursor=<self-position>",
  "first": "http://my-service.bnlapis.com/resources?cursor=<first-position>",
  "prev": "http://my-service.bnlapis.com/resources?cursor=<previous-position>",
  "next": "http://my-service.bnlapis.com/resources?cursor=<next-position>",
  "last": "http://my-service.bnlapis.com/resources?cursor=<last-position>",
  "query": {
    "query-param-<1>": ...,
    "query-param-<n>": ...
  },
  "items": [...]
}
```

**Note**: In case of complex search requests, e.g. when `GET with body` is required, the `cursor` may not be able to encode all query filters. In this case, it is best practice to encode only page position and direction in the `cursor` and transport the query filter in the body - in the request as well as in the response. To protect the pagination sequence, in this case it is recommended, that the `cursor` contains a hash over all applied query filters for pagination request validation.

**Remark**: You should avoid providing a total count unless there is a clear need to do so. Very often, there are significant system and performance implications when supporting full counts. Especially, if the data set grows and requests become complex queries and filters drive full scans. While this is an implementation detail relative to the API, it is important to consider the ability to support serving counts over the life of a service.

## 17. Hypermedia <a id="1700"></a>

### MUST use REST maturity level 2 <a id="1701"></a>

We strive for a good implementation of [REST Maturity Level 2](http://martinfowler.com/articles/richardsonMaturityModel.html#level2) as it enables us to build resource-oriented APIs that make full use of HTTP verbs and status codes. You can see this expressed by many rules throughout these guidelines, e.g.:

- [SHOULD avoid actions — think about resources](#TODO)
- [MUST keep URLs verb-free](#TODO)
- [MUST use HTTP methods correctly](#TODO)
- [MUST use standard HTTP status codes](#TODO)

Although this is not HATEOAS, it should not prevent you from designing proper link relationships in your APIs as stated in rules below.

### MAY use REST maturity level 3 - HATEOAS <a id="1702"></a>

We do not generally recommend to implement [REST Maturity Level 3](http://martinfowler.com/articles/richardsonMaturityModel.html#level3). HATEOAS comes with additional API complexity without real value in a SOA context where client and server interact via REST APIs and provide complex business functions.

Our major concerns regarding the promised advantages of HATEOAS:

- We follow the [API First principle](#TODO) with APIs explicitly defined outside the code with standard specification language. HATEOAS does not really add value for SOA client engineers in terms of API self-descriptiveness: a client engineer finds necessary links and usage description (depending on resource state) in the API reference definition anyway.
- Generic HATEOAS clients which need no prior knowledge about APIs and explore API capabilities based on hypermedia information provided, is a theoretical concept that we haven’t seen working in practice and does not fit to our SOA set-up. The Open API description format (and tooling based on Open API) doesn’t provide sufficient support for HATEOAS either.
- In practice relevant HATEOAS approximations (e.g. following specifications like HAL or JSON API) support API navigation by abstracting from URL endpoint and HTTP method aspects via link types. So, Hypermedia does not prevent clients from required manual changes when domain model changes over time.
- Hypermedia make sense for humans, less for SOA machine clients. We would expect use cases where it may provide value more likely in the frontend and human facing service domain.
- Hypermedia does not prevent API clients to implement shortcuts and directly target resources without 'discovering' them.

However, we do not forbid HATEOAS; you could use it, if you checked its limitations and still see clear value for your usage scenario that justifies its additional complexity.

### MUST use full, absolute URI <a id="1703"></a>

Links to other resource must always use full, absolute URI.

**Motivation**: Exposing any form of relative URI (no matter if the relative URI uses an absolute or relative path) introduces avoidable client side complexity. It also requires clarity on the base URI, which might not be given when using features like embedding subresources. The primary advantage of non-absolute URI is reduction of the payload size, which is better achievable by following the recommendation to use [gzip compression](#TODO)

### MUST use common hypertext controls <a id="1704"></a>

When embedding links to other resources into representations you must use the common hypertext control object. It contains at least one attribute:

- `href`: The URI of the resource the hypertext control is linking to. All our API are using HTTP(s) as URI scheme.

In API that contain any hypertext controls, the attribute name `href` is reserved for usage within hypertext controls.

The schema for hypertext controls can be derived from this model:

```yaml
HttpLink:
  description: A base type of objects representing links to resources.
  type: object
  properties:
    href:
      description: Any URI that is using http or https protocol
      type: string
      format: uri
  required:
    - href
```

The name of an attribute holding such a `HttpLink` object specifies the relation between the object that contains the link and the linked resource. Implementations should use names from the [IANA Link Relation Registry](http://www.iana.org/assignments/link-relations) whenever appropriate. As IANA link relation names use hyphen-case notation, while this guide enforces snake_case notation for attribute names, hyphens in IANA names have to be replaced with underscores (e.g. the IANA link relation type `version-history` would become the attribute `version_history`)

Specific link objects may extend the basic link type with additional attributes, to give additional information related to the linked resource or the relationship between the source resource and the linked one.

E.g. a service providing "Person" resources could model a person who is married with some other person with a hypertext control that contains attributes which describe the other person (`id`, `name`) but also the relationship "spouse" between the two persons (`since`):

```json
{
  "id": "446f9876-e89b-12d3-a456-426655440000",
  "name": "Peter Mustermann",
  "spouse": {
    "href": "https://...",
    "since": "1996-12-19",
    "id": "123e4567-e89b-12d3-a456-426655440000",
    "name": "Linda Mustermann"
  }
}
```

Hypertext controls are allowed anywhere within a JSON model. While this specification would allow HAL, we actually don’t recommend/enforce the usage of [HAL](http://stateless.co/hal_specification.html) anymore as the structural separation of meta-data and data creates more harm than value to the understandability and usability of an API.

### SHOULD use simple hypertext controls for pagination and self-references <a id="1705"></a>

For pagination and self-references a simplified form of the extensible common hypertext controls should be used to reduce the specification and cognitive overhead. It consists of a simple URI value in combination with the corresponding [link relations](http://www.iana.org/assignments/link-relations), e.g. `next`, `prev`, `first`, `last`, or `self`.

See [MUST use common hypertext controls](#TODO) and [SHOULD use pagination links where applicable](#TODO) for more information and examples.

### MUST not use link headers with JSON entities <a id="1706"></a>

For flexibility and precision, we prefer links to be directly embedded in the JSON payload instead of being attached using the uncommon link header syntax. As a result, the use of the `Link` Header defined by [RFC 8288](https://tools.ietf.org/html/rfc8288#section-3) in conjunction with JSON media types is forbidden.

## 18. Common headers <a id="1800"></a>

This section describes a handful of headers, which we found raised the most questions in our daily usage, or which are useful in particular circumstances but not widely known.

### MUST use Content-* headers correctly <a id="1801"></a>

Content or entity headers are headers with a `Content-` prefix. They describe the content of the body of the message and they can be used in both, HTTP requests and responses. Commonly used content headers include but are not limited to:

- `Content-Disposition` can indicate that the representation is supposed to be saved as a file, and the proposed file name.
- `Content-Encoding` indicates compression or encryption algorithms applied to the content.
- `Content-Length` indicates the length of the content (in bytes).
- `Content-Language` indicates that the body is meant for people literate in some human language(s).
- `Content-Location` indicates where the body can be found otherwise ([MAY use Content-Location header for more details](#1803)).
- `Content-Range` is used in responses to range requests to indicate which part of the requested resource representation is delivered with the body.
- `Content-Type` indicates the media type of the body content.

### MAY use standardized headers <a id="1802"></a>

Use [this list](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields) and mention its support in your Open API definition.

### MAY use Content-Location header <a id="1803"></a>

The `Content-Location` header is optional and can be used in successful write operations (`PUT`, `POST`, or `PATCH`) or read operations (`GET`, `HEAD`) to guide caching and signal a receiver the actual location of the resource transmitted in the response body. This allows clients to identify the resource and to update their local copy when receiving a response with this header.

The `Content-Location` header can be used to support the following use cases:

- for reading operations `GET` and `HEAD`, a different location than the requested URI can be used to indicate that the returned resource is subject to content negotiations, and that the value provides a more specific identifier of the resource.
- for writing operations `PUT` and `PATCH`, an identical location to the requested URI can be used to explicitly indicate that the returned resource is the current representation of the newly created or updated resource.
- for writing operations `POST` and `DELETE`, a content location can be used to indicate that the body contains a status report resource in response to the requested action, which is available at provided location.

**Note**: When using the `Content-Location` header, the `Content-Type` header has to be set as well. For example:

```http request
GET /products/123/images HTTP/1.1

HTTP/1.1 200 OK
Content-Type: image/png
Content-Location: /products/123/images?format=raw
```

### SHOULD use Location header instead of Content-Location header <a id="1804"></a>

As the correct usage of `Content-Location` with respect to semantics and caching is difficult, we discourage the use of `Content-Location`. In most cases it is sufficient to direct clients to the resource location by using the `Location` header instead without hitting the `Content-Location` specific ambiguities and complexities.

More details in [RFC 7231 7.1.2 Location](https://tools.ietf.org/html/rfc7231#section-7.1.2), [3.1.4.2 Content-Location](https://tools.ietf.org/html/rfc7231#section-3.1.4.2)

### MAY consider to support Prefer header to handle processing preferences <a id="1805"></a>

The `Prefer` header defined in [RFC 7240](https://tools.ietf.org/html/rfc7240) allows clients to request processing behaviors from servers. It pre-defines a number of preferences and is extensible, to allow others to be defined. Support for the `Prefer` header is entirely optional and at the discretion of API designers, but as an existing Internet Standard, is recommended over defining proprietary "X-" headers for processing directives.

The `Prefer` header can defined like this in an API definition:

```yaml
components:
  headers:
  - Prefer:
      description: >
        The RFC7240 Prefer header indicates that a particular server behavior
        is preferred by the client but is not required for successful completion
        of the request (see [RFC 7240](https://tools.ietf.org/html/rfc7240).
        The following behaviors are supported by this API:

        # (indicate the preferences supported by the API or API endpoint)
        * **respond-async** is used to suggest the server to respond as fast as
          possible asynchronously using 202 - accepted - instead of waiting for
          the result.
        * **return=<minimal|representation>** is used to suggest the server to
          return using 204 without resource (minimal) or using 200 or 201 with
          resource (representation) in the response body on success.
        * **wait=<delta-seconds>** is used to suggest a maximum time the server
          has time to process the request synchronously.
        * **handling=<strict|lenient>** is used to suggest the server to be
          strict and report error conditions or lenient, i.e. robust and try to
          continue, if possible.

      type: array
      items:
        type: string
      required: false
```

**Note**: Please copy only the behaviors into your `Prefer` header specification that are supported by your API endpoint. If necessary, specify different `Prefer` headers for each supported use case.

Supporting APIs may return the `Preference-Applied` header also defined in [RFC 7240](https://tools.ietf.org/html/rfc7240) to indicate whether a preference has been applied.

### MAY consider to support ETag together with If-Match/If-None-Match <a id="1806"></a>

When creating or updating resources it may be necessary to expose conflicts and to prevent the 'lost update' or 'initially created' problem. Following [RFC 7232 "HTTP: Conditional Requests"](https://tools.ietf.org/html/rfc7232) this can be best accomplished by supporting the `ETag` header together with the `If-Match` or `If-None-Match` conditional header. The contents of an `ETag: <entity-tag>` header is either:

- a hash of the response body, 
- a hash of the last modified field of the entity, 
- a version number or identifier of the entity version.

To expose conflicts between concurrent update operations via `PUT`, `POST`, or `PATCH`, the `If-Match: <entity-tag>` header can be used to force the server to check whether the version of the updated entity is conforming to the requested `<entity-tag>`. If no matching entity is found, the operation is supposed a to respond with status code `412` - precondition failed.

Beside other use cases, `If-None-Match: *` can be used in a similar way to expose conflicts in resource creation. If any matching entity is found, the operation is supposed a to respond with status code `412` - precondition failed.

The `ETag`, `If-Match`, and `If-None-Match` headers can be defined as follows in the API definition:

```yaml
components:
  headers:
  - ETag:
      description: |
        The RFC 7232 ETag header field in a response provides the entity-tag of
        a selected resource. The entity-tag is an opaque identifier for versions
        and representations of the same resource over time, regardless whether
        multiple versions are valid at the same time. An entity-tag consists of
        an opaque quoted string, possibly prefixed by a weakness indicator (see
        [RFC 7232 Section 2.3](https://tools.ietf.org/html/rfc7232#section-2.3).

      type: string
      required: false
      example: W/"xy", "5", "5db68c06-1a68-11e9-8341-68f728c1ba70"

  - If-Match:
      description: |
        The RFC7232 If-Match header field in a request requires the server to
        only operate on the resource that matches at least one of the provided
        entity-tags. This allows clients express a precondition that prevent
        the method from being applied if there have been any changes to the
        resource (see [RFC 7232 Section
        3.1](https://tools.ietf.org/html/rfc7232#section-3.1).

      type: string
      required: false
      example: "5", "7da7a728-f910-11e6-942a-68f728c1ba70"

  - If-None-Match:
      description: |
        The RFC7232 If-None-Match header field in a request requires the server
        to only operate on the resource if it does not match any of the provided
        entity-tags. If the provided entity-tag is `*`, it is required that the
        resource does not exist at all (see [RFC 7232 Section
        3.2](https://tools.ietf.org/html/rfc7232#section-3.2).

      type: string
      required: false
      example: "7da7a728-f910-11e6-942a-68f728c1ba70", *
```

Please see [Optimistic locking in RESTful APIs](#TODO) for a detailed discussion and options.

## 19. Proprietary headers <a id="1900"></a>

This section shares definitions of proprietary headers that should be named consistently because they address overarching service-related concerns. Whether services support these concerns or not is optional; therefore, the Open API API specification is the right place to make this explicitly visible. Use the parameter definitions of the resource HTTP methods.

### SHOULD use only the specified proprietary BNL headers <a id="1901"></a>

As a general rule, proprietary HTTP headers should be avoided. From a conceptual point of view, the business semantics and intent of an operation should always be expressed via the URLs path and query parameters, the method, and the content, but not via proprietary headers. Headers are typically used to implement protocol processing aspects, such as flow control, content negotiation, and authentication, and represent business agnostic request modifiers that provide generic context information ([RFC 7231](https://tools.ietf.org/html/rfc7231#section-5)).

However, the exceptional usage of proprietary headers is still helpful when domain-specific generic context information needs to be passed end to end along the service call chain (even if not all called services use it as input for steering service behavior e.g. `x-sales-channel` header)  and/or is provided by specific gateway components, for instance, the API gateway, a Reverse Proxy or sn eventual Monotiring/Tracing product (Splunk and the like).

Below, we explicitly define the list of proprietary header exceptions usable for all services for passing through generic context information of our fashion domain (use case a).

Per convention, non standardized, proprietary header names are prefixed with X-. (Due to backward compatibility, we do not follow the Internet Engineering Task Force’s recommendation in [RFC 6648](https://tools.ietf.org/html/rfc6648) to deprecate usage of X- headers.) Remember that HTTP header field names are not case-sensitive:

| Header field name       | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Header field value example           |
|-------------------------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------|
| `X-Flow-ID`               | `String` | For more information see [MUST support X-Flow-ID](#TODO).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | GKY7oDhpSiKY_gAAAABZ_A               |
| `X-Sales-Channel`         | `String` | Sales channels represent a Distributor sales channel.                                                                                                                                                                                                                                                                                                          | 52b96501-0f8d-43e7-82aa-8a96fab134d7 |
| `X-Frontend-Type`         | `String` | Consumer facing applications (CFAs) provide business experience to their customers via different frontend application types, for instance, mobile app or browser. Info should be passed-through as generic aspect - there are diverse concerns, e.g. pushing mobiles with specific coupons, that make use of it. Current range SHOULD be `mobile-app`, `browser`, `facebook-app`, `chat-app`, etc.                                                                                                                                                                      | mobile-app                           |
| `X-device-Type`           | `String` | There are also use cases for steering customer experience (incl. features and content) depending on device type. Via this header info should be passed-through as generic aspect. Current range SHOULD be `smartphone`, `tablet`, `desktop`, `other`.                                                                                                                                                                                                                                                                                                              | tablet                               |
| `X-device-OS`             | `String` | On top of device type above, we even want to differ between device platform, e.g. smartphone Android vs. iOS. Via this header info should be passed-through as generic aspect. Current range SHOULD be `iOS`, `Android`, `Windows`, `Linux`, `MacOS`.                                                                                                                                                                                                                                                                                                                | Android                              |

**Exception**: The only exception to this guideline are the conventional hop-by-hop` X-RateLimit-` headers which can be used as defined in [MUST use code 429 with headers for rate limits](#TODO).


### MUST propagate proprietary headers <a id="1902"></a>

All BNL’s proprietary headers listed above are end-to-end headers [2](#note-2) and must be propagated to the services down the call chain. The header names and values must remain unchanged.

For example, the values of the custom headers like `X-Device-Type` can affect the results of queries by using device type information to influence recommendation results. Besides, the values of the custom headers can influence the results of the queries (e.g. the device type information influences the recommendation results).

Sometimes the value of a proprietary header will be used as part of the entity in a subsequent request. In such cases, the proprietary headers must still be propagated as headers with the subsequent request, despite the duplication of information.

## 20. API Operation <a id="2000"></a>

### MUST publish Open API specification <a id="2001"></a>

All service applications must publish Open API specifications of their external or internal APIs.

An API is published by copying its **Open API specification** into the reserved `/bnl-apis` (TBD) directory of the deployment artifact used to deploy the provisioning service. The directory must only contain self-contained YAML files that each describe one API (exception see [MUST only use durable and immutable remote references](#303)). 

**Note**: To publish an API, it is still necessary to deploy the artifact successful, as we focus the discovery experience on APIs supported by running services.

### SHOULD monitor API usage <a id="2002"></a>

Owners of APIs used in production should monitor API service to get information about its using clients. This information, for instance, is useful to identify potential review partner for API changes.

Hint: A preferred way of client detection implementation is by logging the client credentials from the security framework (in case of Basic Authentication the `user` should suffice).

## Notes

[1]. <a id="note-1"></a> Per definition of R.Fielding REST APIs have to support HATEOAS (maturity level 3). Our guidelines do not strongly advocate for full REST compliance, but limited hypermedia usage, e.g. for pagination (see [Hypermedia](https://opensource.bnl.com/restful-api-guidelines/#hypermedia)). However, we still use the term "RESTful API", due to the absence of an alternative established term and to keep it like the very majority of web service industry that also use the term for their REST approximations — in fact, in today’s industry full HATEOAS compliant APIs are a very rare exception.

[2]. <a id="note-2"></a> HTTP/1.1 standard ([RFC 7230](https://tools.ietf.org/html/rfc7230#section-6.1)) defines two types of headers: end-to-end and hop-by-hop headers. End-to-end headers must be transmitted to the ultimate recipient of a request or response. Hop-by-hop headers, on the contrary, are meaningful for a single connection only.