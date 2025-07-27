# 📘 Business Context

This document defines essential business and technical terms relevant to the architecture and implementation of a scalable order processing system for a digital marketplace.

---

## Digital Marketplace

A **digital marketplace** is an online platform that connects buyers and sellers to facilitate the exchange of goods or services. The platform typically handles:

- Product listings
- Order placement and tracking
- Payment processing
- Fulfillment and delivery logistics
- Customer and vendor management

Examples: Amazon, Etsy, eBay, App Store, etc.

---

## Scalable

A **scalable** system can handle increased load (e.g., user traffic, transaction volume) by:

- Increasing compute/storage capacity without redesigning architecture
- Maintaining performance under growing demand
- Scaling **horizontally** (adding more instances) or **vertically** (adding resources)

---

## Reliable

A **reliable** system consistently performs as expected, even in the face of:

- Failures (hardware, software, network)
- High usage
- Intermittent service interruptions

Characteristics include:

- Fault tolerance
- Automatic retries
- Durable workflows
- Consistent state management

---

## Resilient

**Resilience** is the ability of a system to recover quickly from failures and continue to operate with minimal downtime or data loss. Techniques include:

- Circuit breakers
- Retry logic with exponential backoff
- Graceful degradation

---

## Maintainable

**Maintainability** refers to the ease of:

- Extending functionality (new payment providers, fulfillment rules)
- Troubleshooting issues
- Updating components with minimal risk
- Onboarding new engineers

Achieved via modular design, documentation, and automated testing.

---

## Cost-Effective

A **cost-effective** system avoids unnecessary complexity and infrastructure bloat. This is vital for startups and scaling companies. Techniques include:

- Pay-as-you-go models (e.g., serverless where appropriate)
- Using managed services vs. building from scratch
- Reusing proven libraries and platforms

---

## Business Profile: Target Client

The proposed solution is designed for a **mid-size digital marketplace** operating in the e-commerce domain, with the following characteristics:

### Current State
- **User Base:** ~100,000 registered users
- **Monthly Active Users (MAU):** ~30,000
- **Current Order Volume:** ~20,000 transactions/month
- **Revenue Model:** Commission on sales (8–12%) + advertising services for sellers
- **Current Tech Stack:** Legacy monolithic system driven via on-premises infrastructure with limited automation in fulfillment and payment processes
- **Fulfillment Model:** Currently, vendors handle their own shipping (drop-shipping model). The marketplace provides only basic order notifications and manual coordination.  
  - No centralized fulfillment or warehouse operations exist yet.
  - **Future Plan:** Introduce optional fulfillment services via partnerships with 3rd-party logistics (3PL) providers to improve delivery speed and customer experience.

### Business Challenges
- Manual workflows leading to **high operational costs**
- Lack of **fault tolerance** (downtime during peak sales) and **durability**
- **Performance degradation** during seasonal spikes
- Limited **integration options** for 3rd-party services (payment providers, logistics)
- Poor **observability**: lack of traces, metrics, health checks

### Business Growth Expectations (3-Year Perspective)
- **User Base:** 5x growth (projected 500,000+)
- **Monthly Order Volume:** 200,000+ transactions
- **Revenue Expansion:** Introduce subscription plans for sellers, targeted advertising

### Key Business Goals
- **Reliability:** Maintain uptime of 99.9% even during peak traffic
- **Scalability:** Support rapid increase in traffic and order volume without full re-architecture
- **Operational Efficiency:** Automate payment settlements, order routing, and fulfillment workflows
- **Analytics & Insights:** Provide near real-time sales and performance dashboards

---

### **Payment Flow (Business Perspective)**
The marketplace must enable buyers to pay for their orders securely and conveniently. Payment can occur **immediately after the buyer confirms the order** or **within the next two days**, depending on the selected payment method and marketplace policy. The order remains reserved during this window, but fulfillment will only start after successful payment.

**Key Business Rules:**
- Payment must be completed before fulfillment begins.
- Accepted methods: major credit/debit cards (future options: bank transfers, digital wallets).
- If payment is not completed within **2 days**, the order is canceled automatically, and the reserved stock is released.
- If payment fails:
  - The buyer is informed immediately.
  - The order remains **unpaid** and may be retried within the allowed window.
- Successful payment changes the order status to **Paid**, making it eligible for fulfillment.

### **Fulfillment Flow (Business Perspective)**
Once an order is **paid**, the marketplace ensures that the products are delivered to the buyer within the agreed timeframe. Fulfillment includes preparing the order, shipping it to the buyer, and providing regular status updates.

**Key Business Rules:**
- Only orders in **Paid** status are eligible for fulfillment.
- The process begins with an **approval step** (e.g., seller approval for custom or high-value items) when applicable.
- Fulfillment involves:
  - Preparing and packaging the products.
  - Arranging shipment through the chosen carrier or logistics provider.
  - Sharing shipment details (e.g., tracking number) with the buyer.
- Buyers must be informed at key stages:
  - Order approved (if required).
  - Order shipped, with estimated delivery date and tracking details.
  - Order delivered.
- If delivery fails:
  - A resolution path is required (e.g., reshipment or refund).
- The marketplace monitors delivery times to ensure SLA compliance.