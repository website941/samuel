# 03. Web Architecture Specification (UK Level 3 Computing)
## Marks and Spencer Group plc

### Architectural Model
Marks and Spencer operates a modern, MACH-compliant (Microservices, API-first, Cloud-native, Headless) e-commerce architecture. Having transitioned from legacy monoliths to a decoupled cloud ecosystem hosted on Microsoft Azure and Google Cloud Platform, the platform easily sustains Black Friday retail traffic surges exceeding 150,000 requests per minute.

### Data Pipeline Flow
Customer Web/App -> Cloudflare Edge CDN & WAF -> Next.js SSR Frontend -> GraphQL Commerce Mesh -> Commercetools & Headless Microservices -> SAP S/4HANA Inventory & Azure Cosmos DB -> Stripe / Worldpay Tokenized Payment Engine.

### Detailed Component Analysis (Grading Criteria)
---------------------------------------------------------
### 1. HTML5 Semantic Elements & Retail Accessibility
- **Technology Stack:** Semantic HTML5, Microdata Product Schema, WAI-ARIA 1.2
- **Technical Role & Function:** Product listing pages (PLP) and product detail pages (PDP) utilize semantic HTML (<article>, <figure>, <figcaption>, <fieldset>) to organize apparel options (size, color, length). Rich Schema.org microdata markup (Product, Offer, AggregateRating) allows search engines to display live stock availability, star ratings, and prices directly in Google search snippets.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Structured schema microdata boosts organic search click-through rates (CTR) and improves screen reader clarity for blind shoppers comparing dress sizes.

```
<div itemscope itemtype="https://schema.org/Product">
  <h1 itemprop="name">Pure Cotton Oxford Shirt</h1>
  <div itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <span itemprop="priceCurrency" content="GBP">£</span><span itemprop="price" content="35.00">35.00</span>
    <link itemprop="availability" href="https://schema.org/InStock" />
  </div>
</div>
```

---------------------------------------------------------
### 2. Cascading Style Sheets (CSS3 & Modern Responsive Design)
- **Technology Stack:** CSS3, Modern CSS Grid, Aspect-Ratio Optimization, Fluid Typography
- **Technical Role & Function:** CSS enforces the refined Marks & Spencer spruce green (#004F32) and stone palette. CSS Grid powers responsive product galleries with aspect-ratio: 3/4 rules to prevent Cumulative Layout Shift (CLS) as high-resolution fashion imagery streams over mobile networks.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Zero layout shift preserves Core Web Vitals, ensuring images do not cause buttons to jump under the user’s finger during checkout.

```
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 1.5rem;
}
.product-image-wrap {
  aspect-ratio: 3 / 4;
  overflow: hidden;
}
```

---------------------------------------------------------
### 3. JavaScript & Interactive Shopping Basket Engine
- **Technology Stack:** TypeScript, ES2022, React 19, Client-Side Cart State, LocalStorage Caching
- **Technical Role & Function:** JavaScript manages dynamic product filters (filter by color, fit, price), instant slide-out shopping baskets, and real-time inventory quantity increments. Client-side state persists uncompleted shopping baskets across browser sessions using encrypted local storage.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Optimistic UI updates update basket badge counts instantly before waiting for server confirmations, delivering a seamless retail experience.

```
// Add to basket with optimistic UI update
function addToBasket(productId: string, size: string, price: number) {
  updateLocalCartState({ productId, size, price, timestamp: Date.now() });
  renderCartDrawer();
  syncWithServerCartAPI(productId, size);
}
```

---------------------------------------------------------
### 4. Headless E-Commerce APIs & GraphQL Mesh
- **Technology Stack:** GraphQL Apollo Gateway, RESTful JSON APIs, Commercetools Microservices
- **Technical Role & Function:** Rather than relying on an inflexible legacy monolithic e-commerce engine, M&S uses a headless GraphQL API mesh. The front-end queries only the exact data required for a specific view (e.g., thumbnail, title, price, size array), minimizing payload sizes over mobile connections.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Decoupling frontend presentation from backend inventory services allows independent deployment cycles without whole-site downtime.

```
query GetProductDetails($sku: String!) {
  product(sku: $sku) {
    title
    price
    availableSizes
    stockLevel
    sparksBonusPoints
  }
}
```

---------------------------------------------------------
### 5. Headless Content Management System (CMS)
- **Technology Stack:** Amplience / Contentful Headless CMS, Dynamic Digital Asset Management (DAM)
- **Technical Role & Function:** Seasonal marketing campaigns (Autumn Fashion, Christmas Foodhall, Summer Dining) are authored inside an enterprise headless CMS. High-resolution campaign photography and video lookbooks are automatically converted into modern WebP and AVIF formats with responsive srcset variations.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Modern image codecs (WebP/AVIF) reduce asset file sizes by up to 40% compared to legacy JPEGs, dramatically improving page load speeds.

```
CMS Pipeline: Editorial Lookbook Published -> Automated WebP Compression -> Edge CDN Invalidation -> Dynamic Homepage Banner Activation.
```

---------------------------------------------------------
### 6. Enterprise Inventory Database Systems
- **Technology Stack:** Azure Cosmos DB, SAP S/4HANA Retail, Redis Enterprise Cluster
- **Technical Role & Function:** Stock levels across all 1,000+ UK stores and fulfillment warehouses are tracked in near-real-time. High-velocity transactional read models reside in Azure Cosmos DB and Redis caches, while core financial ledgers and purchase orders synchronize with SAP S/4HANA.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: In-memory Redis caching allows 50,000 simultaneous shoppers to check whether a coat is in stock at their local Manchester or London branch simultaneously.

```
SELECT store_id, quantity_available FROM store_inventory WHERE product_sku = 'T59/8402' AND store_id = 'LHR-T5' AND quantity_available > 0;
```

---------------------------------------------------------
### 7. Cloud Infrastructure & High-Elasticity Hosting
- **Technology Stack:** Microsoft Azure UK South & Google Cloud Platform (GCP), Kubernetes (AKS)
- **Technical Role & Function:** M&S operates across multi-cloud environments, utilizing Azure for core commerce microservices and GCP for Sparks Big Data machine learning. Kubernetes clusters auto-scale dynamically to absorb massive promotional rushes during Black Friday and seasonal clearance sales.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Automated horizontal pod scaling scales from 30 containers to 450 containers within 120 seconds during flash sale notifications.

```
HorizontalPodAutoscaler (AKS): Metrics: HTTP request rate > 200 req/sec per pod -> Trigger scale out +50 pods.
```

---------------------------------------------------------
### 8. Content Delivery Network (CDN) & Edge Computing
- **Technology Stack:** Cloudflare Enterprise CDN, Anycast Edge Routing, Cloudflare Workers
- **Technical Role & Function:** Cloudflare edge servers distributed across the British Isles cache static product photography, CSS stylesheets, and JavaScript bundles. Edge Workers execute geolocation logic to show nearby store pickup availability before the request reaches the central cloud origin.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Serving 94% of product image requests from edge cache memory slashes origin server bandwidth costs and accelerates load times.

```
Cache-Control: public, max-age=604800, immutable
X-Edge-Cache: HIT-MANCHESTER
```

---------------------------------------------------------
### 9. HTTPS, TLS 1.3 & Payment Security (PCI-DSS)
- **Technology Stack:** TLS 1.3, Strict Transport Security (HSTS), Apple Pay / Google Pay, Tokenized Vaults
- **Technical Role & Function:** All checkout interactions are enforced over TLS 1.3 encryption. Credit card numbers, CVV security codes, and billing addresses are tokenized directly through certified PCI-DSS Level 1 payment processors, ensuring no raw financial details touch M&S application servers.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Tokenization protects consumer credit cards from being compromised even in the hypothetical event of a database dump.

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://js.stripe.com https://applepay.cdn-apple.com; frame-src https://secure.checkout.marksandspencer.com;
```

---------------------------------------------------------
### 10. Authentication & Sparks Customer Identity
- **Technology Stack:** OAuth 2.0, OpenID Connect (OIDC), Biometric WebAuthn Passkeys, JWT
- **Technical Role & Function:** Customers sign in to access their Sparks digital card, order history, and saved payment methods. The identity platform supports modern passwordless WebAuthn biometric logins (Touch ID / Face ID), significantly reducing password reset friction and cart abandonment.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: WebAuthn passkeys are cryptographically immune to traditional phishing scams because the cryptographic private key never leaves the user’s physical phone.

```
POST /api/v1/identity/passkey-verify
{ "credentialId": "A8F9...2B", "authenticatorData": "...", "clientDataJSON": "..." }
```

---------------------------------------------------------
### 11. Continuous Retail Telemetry & Funnel Monitoring
- **Technology Stack:** Dynatrace, Datadog RUM, New Relic, Synthetic Checkout Probes
- **Technical Role & Function:** Real-user monitoring (RUM) tracks every milestone of the digital retail funnel: Search -> Add to Basket -> Checkout -> Payment Success. If the conversion rate at the payment step drops by more than 0.5%, an automated alert pages the e-commerce duty manager.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Synthetic checkout transactions run every 60 seconds using dummy test payment cards to verify that Worldpay and Apple Pay gateways are functioning.

```
Alert Rule: CheckoutConversionRateDrop > 0.5% over 5m -> Dispatch PagerDuty incident to E-Commerce Operations.
```

---------------------------------------------------------
### 12. Disaster Recovery & Redundant Multi-Zone Backups
- **Technology Stack:** Active-Active Multi-Zone Replication, Automated Snapshot Vaulting, RTO/RPO
- **Technical Role & Function:** Order data is mirrored synchronously across isolated availability zones in the UK. In the event of a catastrophic regional failure, traffic automatically switches to secondary cloud data centres in seconds with zero loss of submitted customer orders.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Recovery Time Objective (RTO) is under 5 minutes; Recovery Point Objective (RPO) is zero lost transactions for payment orders.

```
RTO: < 5 mins | RPO: 0 seconds (Synchronous geo-redundant database replication across Azure UK South and UK West).
```

