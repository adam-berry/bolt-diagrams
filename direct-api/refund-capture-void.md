sequenceDiagram
    participant Bolt Backend
    participant SFCC OCAPI Server
    participant SH OMS
    participant Authorize.net
    alt capture authorized payment
    SH OMS-->>+Bolt Backend: POST api.bolt.com/v1/merchant/transactions/capture
    Bolt Backend->>+Authorize.net:POST api.authorize.net/xml/v1/request.api
    Authorize.net-->>-Bolt Backend: Success
    Bolt Backend->>-SH OMS: Success
    else cancel authorization
    SH OMS-->>+Bolt Backend: POST api.bolt.com/v1/merchant/transactions/void
    Bolt Backend->>+Authorize.net:POST api.authorize.net/xml/v1/request.api
    Authorize.net-->>-Bolt Backend: Success
    Bolt Backend->>-SH OMS: Success
    end
    opt refund captured payment
    SH OMS-->>+Bolt Backend: POST api.bolt.com/v1/merchant/transactions/refund
    Bolt Backend->>+Authorize.net:POST api.authorize.net/xml/v1/request.api
    Authorize.net-->>-Bolt Backend: Success
    Bolt Backend->>-SH OMS: Success
    end
