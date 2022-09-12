sequenceDiagram
    actor Shopper
    participant Bolt Frontend
    participant Bolt Backend
    participant SFCC Controller
    participant SFCC OCAPI Server
    participant Authorize.net
    Shopper->>+Bolt Frontend:Select Pay
    Bolt Frontend->>+Bolt Backend:POST /v1/consumer/credit_cards/authorize
    Bolt Backend->>+SFCC OCAPI Server: PUT /baskets/{basket_id}/billing_address
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: GET /baskets/{basket_id}/payment_methods
    SFCC OCAPI Server-->>-Bolt Backend:applicable payment methods
    Bolt Backend->>+SFCC OCAPI Server: POST /baskets/{basket_id}/payment_instruments
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: POST /orders
    Note right of SFCC OCAPI Server: Creates Order Using the Current Basket
    SFCC OCAPI Server-->>-Bolt Backend:order
    Bolt Backend->>+Authorize.net: POST /api.authorize.net/xml/v1/request.api
    Authorize.net-->>-Bolt Backend: Success
    Note left of Bolt Backend: Bolts async risk assessment is enqued
    Bolt Backend->>+SFCC OCAPI Server:PATCH /orders/{order_id}/payment_instruments/{payment_instrument_id}
    Note right of SFCC OCAPI Server: Updates Order and OrderPaymentInstrument custom attributes
    SFCC OCAPI Server-->>-Bolt Backend:order
    Bolt Backend->>+SFCC OCAPI Server:PUT /orders/{order_id}/status
    Note right of SFCC OCAPI Server:SFCC Order Status set to NEW
    SFCC OCAPI Server-->>-Bolt Backend:Success
    Bolt Backend-->>-Bolt Frontend: {tranaction {}, redirect_url }
    Bolt Frontend-->>-Shopper: Redirected to Order Confirmation Page
    Bolt Backend->>+SFCC Controller: /Notification-OrderConfirmEmail
    SFCC Controller-->>-Bolt Backend: Success
    Note left of Bolt Backend: Bolt async risk assessment approves order
    Bolt Backend->>+SFCC OCAPI Server: GET /orders/{order_id}
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server: PATCH /orders/{order_id}
    Note right of SFCC OCAPI Server:Updates order with custom fraud attributes
    SFCC OCAPI Server-->>-Bolt Backend:basket
    Bolt Backend->>+SFCC OCAPI Server:PUT /orders/{order_id}/confirmation_status
    Note right of SFCC OCAPI Server:Sets order Confirmation Status CONFIRMED
    SFCC OCAPI Server-->>-Bolt Backend:Success
    Bolt Backend->>+SFCC OCAPI Server:PUT /orders/{order_id}/export_status
    Note right of SFCC OCAPI Server:Sets order Export Status to READY TO EXPORT
    SFCC OCAPI Server-->>-Bolt Backend:Success
