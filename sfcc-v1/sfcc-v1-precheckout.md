sequenceDiagram
    actor Shopper
    participant Storefront Frontend
    participant Base SFCC Controllers
    participant Bolt Cartridge Controllers
    participant Bolt API
    participant Bolt CDN
    Shopper-->>+Storefront Frontend: Adds item to cart
    Storefront Frontend->>Base SFCC Controllers: POST /Cart-AddProduct
    Base SFCC Controllers->>Bolt Cartridge Controllers: exends
    Bolt Cartridge Controllers-->>Base SFCC Controllers:updated info
    Base SFCC Controllers-->>Storefront Frontend:updated info
    Storefront Frontend->>Base SFCC Controllers: POST /Cart-MiniCartShow
    Base SFCC Controllers->>Bolt Cartridge Controllers: exends
    Bolt Cartridge Controllers->>Bolt API: POST api.bolt.com/v1/merchant/orders
    Bolt API-->>Bolt Cartridge Controllers: Bolt Order Token
    Bolt Cartridge Controllers->>Base SFCC Controllers: render cart/minicart
    Base SFCC Controllers->>Storefront Frontend: render cart/minicart
    opt if Bolt JavaScript has not been downloaded
    Storefront Frontend->>Bolt CDN: requests Bolt's JavaScript
    Bolt CDN-->>Storefront Frontend: connect.bolt.com
    end
    Storefront Frontend->>Storefront Frontend: Executes BoltCheckout.configure() and renders checkout button
    Shopper-->>Storefront Frontend: Selects Bolt Checkout Button
