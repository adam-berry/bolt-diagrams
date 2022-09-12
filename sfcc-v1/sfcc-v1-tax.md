sequenceDiagram
    actor Shopper
    participant SFCC Storefront
    participant SFCC Controller
    participant Bolt Modal
    participant Bolt API 
    Shopper-->>Bolt Modal:Selects a delivery option and selects continue
    Bolt Modal->>Bolt API: /v2/checkout
    Bolt API->>SFCC Controller:POST /Tax-Hook
    SFCC Controller->>Bolt API:bolt.http webservice GET /v1/sfcc_objects/{object_id}
    SFCC Controller->>SFCC Controller:bolt.hook.http webservice POST /Tax-PrepareTaxDetails
    SFCC Controller->>Bolt API: tax data
    Bolt API->>Bolt Modal: order updated with shipping totals
