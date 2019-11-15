---
description: Administer the Connector's routing table.
---

# Routes

Routes help the Connector decide where to forward incoming Interledger packets to. This endpoint supports viewing all of the Connector's routes, and also supports mutating static routes, which take precedence over any dynamic route advertisements that a Connector might receive from its peers in the form of [CCP Route updates](https://github.com/interledger/rfcs/pull/455/files?short_path=deecb3a#diff-deecb3ab70afb68d4a56563d391ccec7).

For more details on the design of the Connector's routing mechanisms, consult[ ](../concepts/ilpv4-routing-table-design.md)the [Routing Table Design](../concepts/ilpv4-routing-table-design.md) section.

## All Routes

{% api-method method="get" host="https://api.cakes.com" path="/routes" %}
{% api-method-summary %}
Get All Routes
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to retrieve a pageable collection of all Routes, whether static or dynamic.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Accept" type="string" required=true %}
Supports \`application/json\` and \`application/hal+json\`
{% endapi-method-parameter %}

{% api-method-parameter name="Authentication" type="string" required=true %}
HTTP Basic Authentication credentials for the Connector admin
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="destinationAddress" type="string" required=true %}
Filters the result by returning a Collection of routes that could be used to route a packet to the specified destination ILP Address.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Routes successfully retrieved.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "_embedded": {
        "routes": [
            {
                "routePrefix": "g",
                "nextHopAccountId": "123",
                "path": [],
                "expiresAt": null,
                "auth": "WhzFqWj0eyDDoq4ynPU3hi6jkon4rwqbpvbFsj+uerw="
            },
            {
                "routePrefix": "g.foo",
                "nextHopAccountId": "123",
                "path": [],
                "expiresAt": null,
                "auth": "YPdfhiJL3niltb1hChYzKwFPPimfPIb0gcNtvoKglsY="
            }
        ]
    },
    "page": {
        "size": "2",
        "totalElements": "2",
        "totalPages": "1",
        "number": "0"
    }
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Not authorized to view Connector Routes
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "title": "Unauthorized",
    "status": 401,
    "detail": "Full authentication is required to access this resource"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Static Routes

{% api-method method="get" host="" path="/routes/static" %}
{% api-method-summary %}
Get Static Routes
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to retrieve a pageable collection of static routes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Supports \`application/json\` and \`application/hal+json\`.
{% endapi-method-parameter %}

{% api-method-parameter name="Accept" type="string" required=true %}
HTTP Basic Authentication credentials for the Connector admin
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "_embedded": {
        "staticRoutes": [
            {
                "createdAt": "2019-11-14T22:31:08.963Z",
                "modifiedAt": "2019-11-14T22:31:08.963Z",
                "routePrefix": "g",
                "nextHopAccountId": "123"
            }
        ]
    },
    "page": {
        "size": "1",
        "totalElements": "1",
        "totalPages": "1",
        "number": "0"
    }
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Not authorized to view static routes
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "title": "Unauthorized",
    "status": 401,
    "detail": "Full authentication is required to access this resource"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="" path="/routes/static/:route\_prefix" %}
{% api-method-summary %}
Create Static Route
{% endapi-method-summary %}

{% api-method-description %}
Create a new static route, identified by \`route\_prefix\`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="route\_prefix" type="string" required=true %}
An Interledger route prefix to identify this static route.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Accept" type="string" required=false %}
Supports \`application/json\`.
{% endapi-method-parameter %}

{% api-method-parameter name="Authentication" type="string" required=true %}
HTTP Basic Authentication credentials for the Connector admin.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="nextHopAccountId" type="string" required=true %}
The account identifier for the next-hop account that packets matching the \`routePrefix\` should be forwarded on.
{% endapi-method-parameter %}

{% api-method-parameter name="routePrefix" type="string" required=true %}
An Interledger Address Prefix that can be used to match against ILP prepare packets' destination address using a longest-prefix match algorithm.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Create a new Static Route
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "createdAt": "2019-11-15T21:37:23.020Z",
    "modifiedAt": "2019-11-15T21:37:23.020Z",
    "routePrefix": "g.foo",
    "nextHopAccountId": "alice-account"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Not authorized to create a new static route.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "title": "Unauthorized",
    "status": 401,
    "detail": "Full authentication is required to access this resource"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="" path="/routes/static/:route\_prefix" %}
{% api-method-summary %}
Delete Static Route
{% endapi-method-summary %}

{% api-method-description %}
Delete the static route identified by \`route\_prefix\`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="routePrefix" type="string" required=true %}
An Interledger Address prefix to identify the static route.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=false %}
HTTP Basic Authentication credentials for the Connector admin.
{% endapi-method-parameter %}

{% api-method-parameter name="Accept" type="string" required=true %}
Supports \`application/json\`.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
The static route was deleted.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No static route existed to be deleted.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "title": "Unauthorized",
    "status": 401,
    "detail": "Full authentication is required to access this resource"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
The static route already existed.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
    {
        "prefix": "g",
        "type": "https://errors.interledger.org/routes/static/static-route-already-exists",
        "title": "Static Route Already Exists (`g`)",
        "status": 409
    }
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

