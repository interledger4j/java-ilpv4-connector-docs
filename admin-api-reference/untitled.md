---
description: Create and manage account on the Connector.
---

# Accounts

{% api-method method="get" host="https://api.connector.com" path="/accounts/:id" %}
{% api-method-summary %}
Get Accounts
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get free cakes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="id" type="string" %}
ID of the account to get.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Accept" type="string" required=false %}
Supports \`application/json\`.
{% endapi-method-parameter %}

{% api-method-parameter name="Authentication" type="string" required=true %}
HTTP Basic Authentication credentials to identify the caller.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Account successfully retrieved.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```javascript
{
    "_embedded": {
        "accounts": [
            {
                "accountId": "__ping_account__",
                "createdAt": "2019-11-14T22:24:29.495Z",
                "modifiedAt": "2019-11-14T22:24:29.495Z",
                "description": "A receiver-like child account for collecting all Ping protocol revenues.",
                "accountRelationship": "CHILD",
                "assetCode": "USD",
                "assetScale": "2",
                "maximumPacketAmount": null,
                "linkType": "PING",
                "ilpAddressSegment": "__ping_account__",
                "connectionInitiator": true,
                "internal": false,
                "sendRoutes": false,
                "receiveRoutes": false,
                "balanceSettings": {
                    "minBalance": null,
                    "settleThreshold": null,
                    "settleTo": "0"
                },
                "rateLimitSettings": {
                    "maxPacketsPerSecond": "1"
                },
                "settlementEngineDetails": null,
                "customSettings": {},
                "_links": {
                    "self": {
                        "href": "http://localhost:8080/accounts/__ping_account__"
                    }
                }
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find an Account with this Id
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```javascript
{
  "title": "Not Found",
  "status": 404,
  "detail": "Account Not Found (`123`)"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="" path="/accounts" %}
{% api-method-summary %}
Create an Account
{% endapi-method-summary %}

{% api-method-description %}
Create a new Account
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="custom\_settings" type="object" required=false %}

{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

    "accountId": "alice",
    "createdAt": "2019-11-15T21:55:42.683Z",
    "modifiedAt": "2019-11-15T21:55:42.683Z",
    "description": "",
    "accountRelationship": "PEER",
    "assetCode": "USD",
    "assetScale": "2",
    "maximumPacketAmount": null,
    "linkType": "ILP_OVER_HTTP",
    "ilpAddressSegment": "alice",
    "connectionInitiator": true,
    "internal": false,
    "sendRoutes": false,
    "receiveRoutes": false,
    "balanceSettings": {
        "minBalance": null,
        "settleThreshold": null,
        "settleTo": "0"
    },
    "rateLimitSettings": {
        "maxPacketsPerSecond": null
    },
    "settlementEngineDetails": null,
    "customSettings": {
        "ilpOverHttp.incoming.auth_type": "JWT_HS_256",
        "ilpOverHttp.incoming.token_issuer": "https://alice.example.com/",
        "ilpOverHttp.incoming.token_audience": "https://connie.example.com/",
        "ilpOverHttp.incoming.token_subject": "alice",
        "ilpOverHttp.incoming.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
        "ilpOverHttp.outgoing.auth_type": "JWT_HS_256",
        "ilpOverHttp.outgoing.token_issuer": "https://connie.example.com/",
        "ilpOverHttp.outgoing.token_audience": "https://alice.example.com/",
        "ilpOverHttp.outgoing.token_subject": "connie",
        "ilpOverHttp.outgoing.shared_secret": "enc:JKS:crypto.p12:secret0:1:aes_gcm:AAAADKZPmASojt1iayb2bPy4D-Toq7TGLTN95HzCQAeJtz0=",
        "ilpOverHttp.outgoing.url": "https://alice.example.com/"
    },
    "_links": {
        "self": {
            "href": "http://localhost:8080/accounts/alice"
        }
    }
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
Not authorized to create an account on this Connector.
{% endapi-method-response-example-description %}

```
{
    "title": "Unauthorized",
    "status": 401,
    "detail": "Full authentication is required to access this resource"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="/accounts/:accountId" %}
{% api-method-summary %}
Get Account
{% endapi-method-summary %}

{% api-method-description %}
Retrieve an account by its unique identifier.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="accountId" type="string" required=false %}
The unique identifier of the account to retrieve.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
```
{
    "accountId": "__ping_account__",
    "createdAt": "2019-11-14T22:24:29.495Z",
    "modifiedAt": "2019-11-14T22:24:29.495Z",
    "description": "A receiver-like child account for collecting all Ping protocol revenues.",
    "accountRelationship": "CHILD",
    "assetCode": "USD",
    "assetScale": "2",
    "maximumPacketAmount": null,
    "linkType": "PING",
    "ilpAddressSegment": "__ping_account__",
    "connectionInitiator": true,
    "internal": false,
    "sendRoutes": false,
    "receiveRoutes": false,
    "balanceSettings": {
        "minBalance": null,
        "settleThreshold": null,
        "settleTo": "0"
    },
    "rateLimitSettings": {
        "maxPacketsPerSecond": "1"
    },
    "settlementEngineDetails": null,
    "customSettings": {},
    "_links": {
        "self": {
            "href": "http://localhost:8080/accounts/__ping_account__"
        }
    }
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
The requested account was not found.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="JSON" %}
    {
        "accountId": "foo",
        "type": "https://errors.interledger.org/accounts/account-not-found",
        "title": "Account Not Found (`foo`)",
        "status": 404
    }
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



