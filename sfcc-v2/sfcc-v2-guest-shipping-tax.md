sequenceDiagram
    actor Shopper
    participant Bolt Frontend
    participant Bolt Backend
    participant SFCC Controller
    participant SFCC OCAPI Server
    participant Authorize.net
    Note over Bolt Frontend,Shopper:Step 1 - Shipping Info
    Shopper->>+Bolt Frontend:Enter Shipping info and selects continue
    Bolt Frontend->>+Bolt Backend:POST updateShippingAddress
    Bolt Backend->>+SFCC OCAPI Server: GET /baskets/{basket_id}
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: PUT /basket/{basket_id}/customer
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: GET /baskets/{basket_id}/shipments/me/shipping_methods
    SFCC OCAPI Server-->>-Bolt Backend: applicable shipping methods
    Bolt Backend-->>-Bolt Frontend:{ shippingOptions [ ] }
    Bolt Frontend-->>-Shopper: Displays Delivery Step with Shipping Options
    Note over Bolt Frontend,Shopper:Step 2 - Delivery Options
    Shopper->>+Bolt Frontend:Select Continue
    Bolt Frontend->>+Bolt Backend:POST fetchTax
    Bolt Backend->>+SFCC OCAPI Server:PUT /baskets/{basket_id}/shipments/me/shipping_method
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend-->>-Bolt Frontend:{ subTotalTax { }, deliveryOption { } }
    Bolt Frontend-->>-Shopper: Displays Payment Step
    Note over Bolt Frontend,Shopper:Step 3 - Payment
    Shopper->>Bolt Frontend:Select Pay
