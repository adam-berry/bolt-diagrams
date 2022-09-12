sequenceDiagram
    actor Shopper
    participant Bolt Frontend
    participant Bolt Backend
    participant SFCC Controller
    participant SFCC OCAPI Server
    participant Authorize.net
    Shopper->>+Bolt Frontend:Launch Checkout
    Bolt Frontend->>+Bolt Backend:POST updateShippingAddress
    Bolt Backend->>+SFCC OCAPI Server: GET /baskets/{basket_id}
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: PUT /basket/{basket_id}/customer
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: GET /baskets/{basket_id}/shipments/me/shipping_methods
    SFCC OCAPI Server-->>-Bolt Backend: applicable shipping methods
    Bolt Backend-->>-Bolt Frontend:{ shippingOptions [ ] }
    Bolt Frontend->>+Bolt Backend:fetchTax
    Bolt Backend->>+SFCC OCAPI Server:PUT /baskets/{basket_id}/shipments/me/shipping_method
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend-->>-Bolt Frontend:{ subTotalTax { }, deliveryOption { } }
    Bolt Frontend-->>-Shopper:Renders 1-Click Checkout
    Shopper->>+Bolt Frontend:Select Pay
