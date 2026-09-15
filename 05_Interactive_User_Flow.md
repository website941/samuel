# 05. User Experience Flow Architecture
## Marks and Spencer Group plc - Scenario: Online Fashion Product Search, Sparks Discount & Click & Collect Flow

**Objective:** Map the complete customer journey of an online shopper browsing autumn clothing, checking real-time store stock for click & collect, redeeming a Sparks loyalty voucher, and completing secure tokenized payment.

### End-to-End Step-by-Step Architecture
#### Step 1: Customer Browses Fashion & Applies Faceted Filters
- **Actor:** User
- **User Action:** Navigates to marksandspencer.com, filters womenswear by "Pure Cotton", Size "12", and Colour "Navy".
- **Technical Execution & API Responses:** Frontend calls GraphQL product catalogue API with faceted filter parameters, returning matching SKUs with thumbnail images.


#### Step 2: Real-Time Store Inventory Lookup for Click & Collect
- **Actor:** Client Frontend
- **User Action:** Customer enters postcode "M1 1AD" to check local store collection availability.
- **Technical Execution & API Responses:** Calls SAP S/4HANA stock API via Redis cache layer; confirms item is eligible for Free Next-Day Click & Collect at Manchester Market Street store.


#### Step 3: Item Added to Basket & Cart State Updated
- **Actor:** Client Frontend
- **User Action:** Clicks "Add to Bag" for Pure Cotton Navy Trench Coat (£79.00).
- **Technical Execution & API Responses:** Client-side cart state updates optimistically; sends encrypted asynchronous POST to /api/cart/items to lock warehouse inventory for 15 minutes.


#### Step 4: Customer Authentication & Sparks Voucher Application
- **Actor:** Backend Service
- **User Action:** Customer signs in with Sparks account using biometric WebAuthn or password.
- **Technical Execution & API Responses:** Server retrieves valid Sparks discount token (20% off outerwear) and recalculates order subtotal to £63.20.


#### Step 5: PCI-DSS Tokenized Checkout & Apple Pay / Card Processing
- **Actor:** Database / External
- **User Action:** Customer authorizes payment via Apple Pay or tokenized card iframe.
- **Technical Execution & API Responses:** Payment token dispatched to Stripe/Worldpay gateway with 3D Secure 2.0 authorization; zero cardholder data touches M&S servers.
- **Exception Handling Path:** If card authorization fails, customer is prompted to retry with an alternate card or PayPal.

#### Step 6: Order Confirmation & Click & Collect QR Code Generated
- **Actor:** Backend Service
- **User Action:** Generates order confirmation number (e.g. MS-UK-8492019) and dispatches collection barcode to customer email and app.
- **Technical Execution & API Responses:** Order pushed to Castle Donington fulfillment warehouse management system (WMS) for overnight store dispatch.


