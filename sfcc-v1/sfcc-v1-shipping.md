sequenceDiagram
    actor Shopper
    participant SFCC Storefront
    participant SFCC Controller
    participant Bolt Modal
    participant Bolt API 
    Shopper-->>Bolt Modal: Enters delivery information and selects continue
    Bolt Modal->>Bolt API: /v2/checkout
    Bolt API->>SFCC Controller:POST /Shipping-Hook
    SFCC Controller->>Bolt API:bolt.http webservice GET /v1/sfcc_objects/{object_id}
    SFCC Controller->>SFCC Controller:bolt.hook.http webservice POST /Shipping-PrepareShippingDetails
    SFCC Controller->>Bolt API: delivery options
    Bolt API->>Bolt Modal: displays delivery options to shopper
