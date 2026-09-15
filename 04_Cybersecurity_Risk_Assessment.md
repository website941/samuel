# 04. Cybersecurity Risk Assessment & UK Legal Frameworks
## Marks and Spencer Group plc

### Security Philosophy & Industry Standards
As one of Britain’s largest retail e-commerce operations, Marks & Spencer is targeted by cybercriminals attempting customer account takeover, Sparks loyalty points theft, fraudulent refund schemes, payment card skimming, and counterfeit promotional voucher scams.

### Compliance Standards & Frameworks
- UK General Data Protection Regulation (UK GDPR)
- Data Protection Act 2018
- PCI-DSS Level 1 Compliance (Payment Card Industry Security Standards)
- Consumer Rights Act 2015
- Cyber Essentials Plus Certification
- ISO/IEC 27001 Information Security Management

### Threat Risk Matrix
---------------------------------------------------------
### Threat 1: Fake Discount Voucher & Phishing Clone Websites
- **Risk Severity Level:** CRITICAL
- **Attack Vector:** Social media phishing ads, typo-squatted domains (e.g. marks-spencer-clearance-sale.co.uk), WhatsApp scam messages.
- **Operational & Reputational Impact:** Scammers create social media ads and lookalike websites advertising "M&S 50% Off Everything Closing Down Clearance", duping unsuspecting customers into submitting credit card numbers on fraudulent clone portals.
- **Technical Mitigation Strategy:** Aggressive trademark monitoring and swift domain takedowns via BrandShield and NCSC, public social media alerts educating customers that authentic M&S offers only originate from marksandspencer.com, and strict DMARC p=reject email enforcement.
- **Applicable UK Legal Statute:** Fraud Act 2006 Section 2, Trademarks Act 1994.

---------------------------------------------------------
### Threat 2: Sparks Loyalty Account Takeover & Points Theft
- **Risk Severity Level:** HIGH
- **Attack Vector:** Automated credential stuffing utilizing stolen password databases from unrelated third-party breaches.
- **Operational & Reputational Impact:** Criminal syndicates deploy automated credential stuffing to breach customer M&S accounts, draining accumulated Sparks reward vouchers or stored e-gift card balances.
- **Technical Mitigation Strategy:** Behavioral bot mitigation blocking automated login attempts, mandatory two-factor authentication for high-value gift card transactions, and integration with "Have I Been Pwned" to reject compromised passwords.
- **Applicable UK Legal Statute:** Computer Misuse Act 1990 Section 1, UK GDPR Article 32.

---------------------------------------------------------
### Threat 3: Cross-Site Scripting (XSS) & Digital Card Skimming
- **Risk Severity Level:** HIGH
- **Attack Vector:** Stored or reflected XSS vulnerabilities, compromised third-party analytics tag containers.
- **Operational & Reputational Impact:** Malicious script injection into product review modules or third-party marketing tags could attempt to capture customer credit card numbers or address details during checkout.
- **Technical Mitigation Strategy:** Strict Content Security Policy (CSP) blocking unauthorized scripts, Subresource Integrity (SRI) on external libraries, and tokenized payment iframes isolated from parent page JavaScript.
- **Applicable UK Legal Statute:** PCI-DSS Requirement 6.4.3, Data Protection Act 2018.

---------------------------------------------------------
### Threat 4: Inaccurate Stock Information & Inventory Hoarding Bots
- **Risk Severity Level:** HIGH
- **Attack Vector:** Automated headless browser scripts executing rapid cart reservations.
- **Operational & Reputational Impact:** Scalper botnets rapidly add limited-edition fashion or Christmas Foodhall items to shopping baskets without purchasing, falsely indicating items are "Out of Stock" to genuine retail customers and disrupting warehouse logistics.
- **Technical Mitigation Strategy:** Cloudflare Bot Management identifying non-human browser behavior, rate-limiting cart reservation endpoints, and imposing a 15-minute basket release timer for unpurchased items.
- **Applicable UK Legal Statute:** Computer Misuse Act 1990 Section 3, Consumer Protection from Unfair Trading Regulations 2008.

---------------------------------------------------------
### Threat 5: Website Downtime During Black Friday & Peak Trading Hours
- **Risk Severity Level:** HIGH
- **Attack Vector:** Massive organic traffic spikes combined with opportunistic Layer 7 DDoS attacks.
- **Operational & Reputational Impact:** Unplanned website outages on Cyber Monday or Christmas Eve can result in hundreds of thousands of pounds of lost retail revenue every hour and damage brand reputation.
- **Technical Mitigation Strategy:** Dynamic auto-scaling Kubernetes clusters, origin-shielding CDN caching, queue management waiting rooms during extreme demand, and multi-region disaster recovery failover.
- **Applicable UK Legal Statute:** Consumer Rights Act 2015.


### NCSC 5-Stage Incident Response Plan
Marks and Spencer maintains a Computer Security Incident Response Team (CSIRT) operating 24/7/365. The incident response protocol establishes immediate threat containment, engages third-party forensic specialists within 1 hour, enforces mandatory credential revocations if user accounts are suspect, and complies strictly with the statutory 72-hour ICO notification threshold under UK GDPR.
