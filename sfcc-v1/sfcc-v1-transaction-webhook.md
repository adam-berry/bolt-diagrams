sequenceDiagram
    participant Bolt Server
    participant Bolt SFCC Cartridge
    Note left of Bolt Server: A transaction's (payment's) status is updated. 
    Note left of Bolt Server: A webhook update is enqued to update order in SFCC 
    Bolt Server->>Bolt SFCC Cartridge: POST https://www.forever21.com/on/demandware.store/Sites-forever21-Site/en_US/Webhook-V2
    Note right of Bolt SFCC Cartridge: Creates a new webservice http request
    Bolt SFCC Cartridge->>Bolt SFCC Cartridge: POST https://www.forever21.com/on/demandware.store/Sites-forever21-Site/en_US/Webhook-Transaction
    Note right of Bolt SFCC Cartridge: Updates the SFCC order data based on the type of transaction update
    Bolt SFCC Cartridge->>Bolt Server: 200 Success 
    Note left of Bolt Server: The webhook is removed from webhook queue
