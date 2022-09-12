sequenceDiagram
    actor Shopper
    participant SFCC Storefront
    participant SFCC Controller
    participant Bolt Modal
    participant Bolt API 
    Shopper-->>SFCC Storefront: Adds item to cart
    SFCC Storefront->>SFCC Controller: POST /Cart-AddProduct
    Shopper-->>SFCC Storefront: Navigate to cart page
    SFCC Storefront->>SFCC Controller: GET /Cart-Show
    SFCC Controller->>Bolt API: POST /v1/merchant/orders
    SFCC Controller->>Bolt API: POST /v1/sfcc_objects (sfcc basket and dwsid)
    SFCC Controller->>SFCC Storefront: render cart veiw template
    SFCC Storefront->>Bolt API: load track.js, connect.js, configure Bolt Checkout, GET /v1/orders
    Shopper-->>SFCC Storefront: Selects the Bolt Checkout Button

