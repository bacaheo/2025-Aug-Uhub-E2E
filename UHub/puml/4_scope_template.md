| No. | Function | Scope of work |
| --- | --- | --- |
| 1 | Foundation & Infrastructure | - Thiết lập Azure App Service environment (Dev/Staging/Prod)<br>- CI/CD pipeline configuration với automated testing<br>- Database schema tạo với audit trail tables<br>- Basic authentication framework (.NET Core + OAuth 2.0)<br>- Health check endpoints và system monitoring |
| 2 | UGMS → UHub API Integration | Automated sync of GiftCode, Scheme, Customer, Ship_to data eliminating manual data entry and ensuring accuracy<br>- RSA 2048-bit authentication cho UGMS API calls<br>- API client library với retry logic và error handling<br>- Data mapping layer giữa UGMS và UHub models<br>- Webhook endpoint để nhận UGMS notifications<br>- Automatic sync Schemes, Gift codes, Quantities |
| 3 | Allocation Management | Store-level allocation tracking với digital confirmation và adjustment capability |
| 4 | Basic Audit Trail | Complete transaction logging for compliance with evidence linking and timestamp tracking |
| 5 | Enhanced Ticket System | Digital workflow with SLA tracking, auto-expiry (24h Agency, 12h Sales), và approval loop management |