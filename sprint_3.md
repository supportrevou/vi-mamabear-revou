# 🛒 Sprint 3: The Transaction Engine

## Sprint Overview

**Duration:** 2 Weeks  
**Sprint Goal:** Convert visitors into customers via cart and checkout functionality  
**Status:** Planning  
**Sprint Period:** Week 5-6

---

## 🎯 Sprint Objectives

1. Implement fully functional shopping cart system
2. Build complete checkout flow with shipping integration
3. Integrate payment gateway for transaction processing
4. Develop order management system with inventory control
5. Enable real-time shipping cost calculation
6. Create order confirmation and tracking system

---

## 📋 Sprint Backlog

### Front-End Development Tasks

#### 1. Shopping Cart Functionality
- [ ] Create shopping cart page (`/cart`)
  - Cart items list with product details
  - Product image, name, variant, price
  - Quantity selector with stock validation
  - Remove item button
  - Update quantity with real-time price calculation
  - Empty cart state with CTA to browse products
- [ ] Implement cart state management
  - Add to cart functionality
  - Update cart item quantity
  - Remove from cart
  - Clear entire cart
  - Persist cart data (localStorage for guests, database for logged-in users)
  - Sync cart between devices (for logged-in users)
- [ ] Create cart summary component
  - Subtotal calculation
  - Shipping cost (to be calculated at checkout)
  - Tax calculation (if applicable)
  - Total amount
  - Discount/promo code input (UI only for now)
  - "Proceed to Checkout" button
- [ ] Implement cart icon in navbar
  - Item count badge
  - Mini cart dropdown (quick view)
  - Quick remove from mini cart
- [ ] Add cart notifications
  - "Added to cart" toast notification
  - "Item removed" notification
  - Stock warning notifications
- [ ] Implement cart validation
  - Check stock availability before checkout
  - Handle out-of-stock items
  - Price change notifications

**Estimated Effort:** 20 hours  
**Priority:** Critical  
**Dependencies:** Sprint 2 Product APIs

#### 2. Checkout Flow UI
- [ ] Create multi-step checkout page (`/checkout`)
  - Step 1: Shipping Information
  - Step 2: Shipping Method
  - Step 3: Payment Method
  - Step 4: Order Review
  - Progress indicator
- [ ] Build shipping information form
  - Full name, phone number
  - Address fields (street, city, province, postal code)
  - Address validation
  - Save address option (for logged-in users)
  - Multiple address selection (for returning customers)
  - Add new address option
- [ ] Implement shipping method selection
  - Display available couriers (JNE, TIKI, POS, etc.)
  - Show shipping cost for each option
  - Estimated delivery time
  - Service type selection (Regular, Express, etc.)
  - Loading state while fetching shipping rates
- [ ] Create payment method selection
  - Display available payment options
  - Bank transfer instructions
  - E-wallet options (GoPay, OVO, Dana)
  - Credit/debit card form
  - Virtual account options
  - Payment gateway integration UI
- [ ] Build order review section
  - Order summary (items, quantities, prices)
  - Shipping address confirmation
  - Shipping method confirmation
  - Payment method confirmation
  - Terms and conditions checkbox
  - "Place Order" button with loading state
- [ ] Implement checkout validation
  - Validate all required fields
  - Verify stock availability before order placement
  - Validate shipping address
  - Ensure payment method is selected
- [ ] Create order confirmation page
  - Order number display
  - Order details summary
  - Payment instructions (if applicable)
  - Estimated delivery date
  - "Track Order" button
  - "Continue Shopping" button

**Estimated Effort:** 28 hours  
**Priority:** Critical  
**Dependencies:** Sprint 3 BE Shipping & Payment APIs

#### 3. Search & Filter Integration
- [ ] Complete search functionality integration
  - Connect search bar to search API
  - Display search results
  - Handle no results state
  - Implement search suggestions
- [ ] Complete filter integration
  - Connect all filter options to API
  - Update URL with filter parameters
  - Handle filter combinations
  - Show active filters
  - Clear filters functionality
- [ ] Implement sorting integration
  - Connect sorting dropdown to API
  - Update results based on sort selection
  - Maintain sort state in URL
- [ ] Add loading states for search and filter
- [ ] Optimize search and filter performance
  - Debounce search input
  - Lazy load filter options
  - Cache filter results

**Estimated Effort:** 12 hours  
**Priority:** High  
**Dependencies:** Sprint 2 Search API

#### 4. User Account - Order History
- [ ] Create order history page (`/account/orders`)
  - List all user orders
  - Order card with key information
  - Order status badge
  - Order date and total
  - "View Details" button
- [ ] Create order detail page (`/account/orders/[id]`)
  - Complete order information
  - Order items list
  - Shipping address
  - Shipping tracking information
  - Payment status
  - Order timeline/status history
  - "Cancel Order" button (if applicable)
  - "Download Invoice" button
- [ ] Implement order filtering
  - Filter by status (pending, processing, shipped, delivered, cancelled)
  - Filter by date range
  - Search by order number
- [ ] Add order tracking integration
  - Display tracking number
  - Show current shipping status
  - Estimated delivery date
  - Tracking history

**Estimated Effort:** 16 hours  
**Priority:** Medium  
**Dependencies:** Sprint 3 Order API

---

### Back-End Development Tasks

#### 1. Shipping API Integration (RajaOngkir)
- [ ] Set up RajaOngkir API account and credentials
- [ ] Create shipping service module
  - Get available couriers
  - Calculate shipping cost
  - Get province list
  - Get city list
  - Get subdistrict list (if needed)
- [ ] Implement shipping API endpoints:
  - `GET /api/shipping/provinces` - Get all provinces
  - `GET /api/shipping/cities?province_id={id}` - Get cities by province
  - `POST /api/shipping/cost` - Calculate shipping cost
    - Parameters: origin, destination, weight, courier
    - Return: available services with costs and ETD
  - `GET /api/shipping/couriers` - Get available couriers
- [ ] Implement shipping cost calculation logic
  - Calculate total weight from cart items
  - Get shipping rates for multiple couriers
  - Cache shipping rates (short TTL)
  - Handle API errors gracefully
- [ ] Add shipping address validation
  - Verify province and city IDs
  - Validate postal code format
  - Check address completeness
- [ ] Create shipping configuration
  - Set origin address (warehouse/store location)
  - Configure available couriers
  - Set weight calculation rules
- [ ] Implement error handling for shipping API
  - Handle API timeouts
  - Fallback for API failures
  - Log shipping API errors

**Estimated Effort:** 18 hours  
**Priority:** Critical  
**Dependencies:** Sprint 2 Product Schema (weight field)

#### 2. Payment Gateway Integration (Midtrans/Xendit)
- [ ] Choose payment gateway (Midtrans recommended)
- [ ] Set up payment gateway account (sandbox for testing)
- [ ] Configure payment gateway credentials
- [ ] Implement payment service module
  - Create payment transaction
  - Handle payment notification (webhook)
  - Check payment status
  - Cancel payment
  - Refund payment (prepare for future)
- [ ] Create payment API endpoints:
  - `POST /api/payments/create` - Create payment transaction
  - `POST /api/payments/notification` - Handle payment webhook
  - `GET /api/payments/:id/status` - Check payment status
  - `POST /api/payments/:id/cancel` - Cancel payment
- [ ] Implement payment methods
  - Bank transfer (Virtual Account)
  - E-wallets (GoPay, OVO, Dana, LinkAja)
  - Credit/debit card
  - Convenience store (Alfamart, Indomaret)
  - QRIS
- [ ] Build payment webhook handler
  - Verify webhook signature
  - Update order status based on payment status
  - Send confirmation email
  - Update inventory
  - Log payment events
- [ ] Implement payment security
  - Validate payment amounts
  - Verify transaction authenticity
  - Prevent duplicate payments
  - Secure webhook endpoint
- [ ] Add payment status tracking
  - Pending
  - Processing
  - Success
  - Failed
  - Expired
  - Cancelled

**Estimated Effort:** 24 hours  
**Priority:** Critical  
**Dependencies:** Sprint 3 Order System

#### 3. Order & Inventory Management
- [ ] Design Order database schema
  - id, orderNumber, userId, status
  - subtotal, shippingCost, tax, total
  - shippingAddress (JSON or separate table)
  - shippingMethod, trackingNumber
  - paymentMethod, paymentStatus
  - notes, createdAt, updatedAt
- [ ] Design OrderItem schema
  - id, orderId, productId, variantId
  - productName, variantName (snapshot)
  - quantity, price, subtotal
- [ ] Design OrderStatusHistory schema
  - id, orderId, status, notes, createdAt
- [ ] Create Prisma migrations for order tables
- [ ] Implement Order API endpoints:
  - `POST /api/orders` - Create new order
  - `GET /api/orders` - Get user orders (with pagination)
  - `GET /api/orders/:id` - Get order details
  - `PUT /api/orders/:id/status` - Update order status (admin)
  - `POST /api/orders/:id/cancel` - Cancel order
  - `GET /api/orders/:id/invoice` - Generate invoice
- [ ] Implement order creation logic
  - Validate cart items and stock
  - Calculate totals (subtotal, shipping, tax, total)
  - Generate unique order number
  - Create order and order items
  - Reduce product inventory
  - Create payment transaction
  - Send order confirmation email
  - Clear user cart
- [ ] Implement inventory management
  - Reduce stock on order creation
  - Restore stock on order cancellation
  - Handle low stock alerts
  - Prevent overselling (stock validation)
  - Track inventory changes
- [ ] Implement order status workflow
  - Pending (waiting for payment)
  - Paid (payment confirmed)
  - Processing (preparing order)
  - Shipped (order dispatched)
  - Delivered (order received)
  - Cancelled (order cancelled)
  - Refunded (payment refunded)
- [ ] Add order validation
  - Minimum order amount (if applicable)
  - Maximum order quantity per product
  - Validate shipping address
  - Verify payment method availability
- [ ] Implement order search and filtering
  - Search by order number
  - Filter by status
  - Filter by date range
  - Filter by customer

**Estimated Effort:** 28 hours  
**Priority:** Critical  
**Dependencies:** Sprint 2 Product & User APIs

#### 4. Shopping Cart API
- [ ] Design Cart database schema
  - id, userId (nullable for guest carts), sessionId
  - createdAt, updatedAt, expiresAt
- [ ] Design CartItem schema
  - id, cartId, productId, variantId
  - quantity, price (snapshot), createdAt
- [ ] Implement Cart API endpoints:
  - `GET /api/cart` - Get user cart
  - `POST /api/cart/items` - Add item to cart
  - `PUT /api/cart/items/:id` - Update cart item quantity
  - `DELETE /api/cart/items/:id` - Remove item from cart
  - `DELETE /api/cart` - Clear entire cart
  - `POST /api/cart/merge` - Merge guest cart with user cart (on login)
- [ ] Implement cart business logic
  - Create cart for new users/sessions
  - Validate product and variant existence
  - Check stock availability
  - Update cart totals
  - Handle price changes
  - Expire old carts (cleanup job)
- [ ] Implement cart validation
  - Validate quantity (min: 1, max: stock)
  - Check product availability
  - Verify variant compatibility
  - Validate cart before checkout
- [ ] Add cart optimization
  - Cache cart data
  - Optimize cart queries (eager loading)
  - Implement cart item deduplication

**Estimated Effort:** 16 hours  
**Priority:** High  
**Dependencies:** Sprint 2 Product APIs

#### 5. Email Notification System
- [ ] Set up email service (SendGrid, AWS SES, or Nodemailer)
- [ ] Create email templates
  - Order confirmation email
  - Payment confirmation email
  - Shipping notification email
  - Delivery confirmation email
  - Order cancellation email
- [ ] Implement email service module
  - Send email function
  - Template rendering
  - Error handling and retry logic
- [ ] Create email queue system (optional but recommended)
  - Use Bull or similar queue library
  - Handle email sending asynchronously
  - Retry failed emails
- [ ] Implement email triggers
  - Send on order creation
  - Send on payment confirmation
  - Send on order status change
  - Send on order cancellation
- [ ] Add email logging
  - Track sent emails
  - Log email failures
  - Monitor email delivery rates

**Estimated Effort:** 12 hours  
**Priority:** Medium  
**Dependencies:** Sprint 3 Order System

---

## 🔗 Integration Tasks

### Front-End + Back-End Integration
- [ ] Integrate shopping cart with cart API
- [ ] Connect checkout flow with shipping API
- [ ] Integrate payment gateway with frontend
- [ ] Connect order creation with backend
- [ ] Implement payment webhook handling
- [ ] Test complete purchase flow end-to-end
- [ ] Verify email notifications are sent
- [ ] Test order status updates
- [ ] Validate inventory reduction
- [ ] Test error scenarios (payment failure, out of stock, etc.)

**Estimated Effort:** 16 hours  
**Priority:** Critical  
**Dependencies:** All FE and BE tasks

---

## 📊 Definition of Done

### For Each User Story:
- [ ] Code is written and follows project coding standards
- [ ] Code is reviewed by at least one team member
- [ ] Unit tests are written and passing (BE)
- [ ] Integration tests are passing
- [ ] Payment gateway tested in sandbox mode
- [ ] API documentation is updated
- [ ] Feature is tested on multiple browsers
- [ ] Feature is tested on mobile devices
- [ ] Security review completed
- [ ] No critical bugs or vulnerabilities
- [ ] Code is merged to main/develop branch

### For Sprint 3 Completion:
- [ ] Users can add products to cart
- [ ] Users can complete checkout process
- [ ] Shipping costs are calculated correctly
- [ ] Payment gateway is working (sandbox)
- [ ] Orders are created successfully
- [ ] Inventory is updated on order creation
- [ ] Email notifications are sent
- [ ] Order history is accessible
- [ ] Complete purchase flow tested end-to-end
- [ ] Sprint Review demo is prepared
- [ ] Documentation is updated

---

## 🎨 Design Guidelines

### Shopping Cart
- **Layout:** List view with clear item separation
- **Actions:** Prominent update and remove buttons
- **Summary:** Sticky cart summary on desktop
- **Empty State:** Friendly message with product suggestions
- **Mobile:** Swipe to delete functionality

### Checkout Flow
- **Progress:** Clear step indicator at top
- **Forms:** Single column, well-spaced fields
- **Validation:** Inline error messages
- **CTA:** Large, prominent "Continue" and "Place Order" buttons
- **Trust Signals:** Security badges, SSL indicators

### Order Confirmation
- **Hierarchy:** Order number most prominent
- **Information:** Clear, scannable layout
- **Actions:** Clear next steps (payment instructions, tracking)
- **Reassurance:** Confirmation message and email notification

---

## 🔐 Security Considerations

### Payment Security
- **PCI Compliance:** Use payment gateway's hosted payment page (no card data on server)
- **Webhook Security:** Verify webhook signatures
- **Amount Validation:** Always validate payment amounts server-side
- **Fraud Prevention:** Implement basic fraud checks
- **Secure Communication:** Use HTTPS for all payment-related requests
- **Token Security:** Secure payment tokens and transaction IDs

### Order Security
- **Authorization:** Users can only access their own orders
- **Price Integrity:** Validate prices server-side (don't trust client)
- **Stock Validation:** Check stock availability before order creation
- **Idempotency:** Prevent duplicate order creation
- **Input Validation:** Sanitize all order inputs
- **Rate Limiting:** Limit order creation attempts

### Cart Security
- **Session Security:** Secure cart session tokens
- **Price Validation:** Always use server-side prices
- **Stock Validation:** Verify stock before checkout
- **Cart Expiration:** Expire old carts to free resources

---

## 🚧 Known Risks & Mitigation

| Risk | Impact | Probability | Mitigation Strategy |
|------|--------|-------------|---------------------|
| Payment gateway integration complexity | Critical | High | Start early; use comprehensive documentation; test thoroughly in sandbox |
| Shipping API reliability issues | High | Medium | Implement caching; have fallback shipping rates |
| Inventory race conditions | High | Medium | Use database transactions; implement pessimistic locking |
| Payment webhook failures | Critical | Low | Implement retry mechanism; manual reconciliation process |
| Email delivery failures | Medium | Medium | Use reliable email service; implement queue with retries |
| Checkout flow too complex | High | Low | User testing; simplify steps if needed |
| Currency/price calculation errors | Critical | Low | Thorough testing; use decimal types for money |

---

## 📅 Sprint Schedule

### Week 1
**Days 1-2:** Shopping cart API and UI; Shipping API integration  
**Days 3-4:** Payment gateway setup; Order system backend  
**Day 5:** Cart and shipping integration testing

### Week 2
**Days 1-2:** Checkout flow UI; Payment integration  
**Days 3-4:** Order completion flow; Email notifications  
**Day 5:** End-to-end testing, bug fixes, sprint review preparation

---

## 🤝 Team Collaboration

### Critical Integration Points
- **Day 3:** Cart API ready for frontend integration
- **Day 5:** Shipping API ready for checkout integration
- **Day 7:** Payment gateway ready for testing
- **Day 9:** Order API ready for frontend integration
- **Day 10:** Complete purchase flow ready for end-to-end testing

### Testing Strategy
- Unit tests for all business logic
- Integration tests for payment and shipping APIs
- End-to-end tests for complete purchase flow
- Manual testing of payment scenarios
- Security testing of payment and order endpoints

---

## 📚 Resources & References

### Documentation
- [Midtrans Documentation](https://docs.midtrans.com/)
- [Xendit Documentation](https://developers.xendit.co/)
- [RajaOngkir API Documentation](https://rajaongkir.com/)
- [Stripe Payment Intents](https://stripe.com/docs/payments/payment-intents) (for reference)

### Tools
- **Payment Testing:** Midtrans Sandbox, Xendit Test Mode
- **Email Testing:** Mailtrap, MailHog
- **API Testing:** Postman, Insomnia
- **Load Testing:** Artillery, k6 (for future)

---

## 🧪 Testing Checklist

### Shopping Cart Testing
- [ ] Add item to cart
- [ ] Update item quantity
- [ ] Remove item from cart
- [ ] Clear entire cart
- [ ] Cart persists on page refresh
- [ ] Cart syncs on login
- [ ] Stock validation works
- [ ] Price updates correctly

### Checkout Testing
- [ ] All form validations work
- [ ] Shipping cost calculates correctly
- [ ] Multiple shipping options display
- [ ] Payment methods display correctly
- [ ] Order review shows correct information
- [ ] Order creation succeeds
- [ ] Inventory reduces correctly
- [ ] Email notification sent

### Payment Testing
- [ ] Test all payment methods in sandbox
- [ ] Successful payment flow
- [ ] Failed payment handling
- [ ] Expired payment handling
- [ ] Webhook processing
- [ ] Payment status updates correctly
- [ ] Duplicate payment prevention

### Order Testing
- [ ] Order appears in order history
- [ ] Order details display correctly
- [ ] Order status updates work
- [ ] Order cancellation works
- [ ] Invoice generation works
- [ ] Tracking information displays

---

## 📝 Sprint Retrospective Template

### What Went Well?
- (To be filled at end of sprint)

### What Could Be Improved?
- (To be filled at end of sprint)

### Action Items for Next Sprint
- (To be filled at end of sprint)

### Key Learnings
- (To be filled at end of sprint)

### Technical Debt Identified
- (To be filled at end of sprint)

---

## ✅ Sprint Review Checklist

- [ ] Demo complete shopping cart functionality
- [ ] Show checkout flow with shipping calculation
- [ ] Demonstrate payment gateway integration (sandbox)
- [ ] Show order creation and confirmation
- [ ] Demo email notifications
- [ ] Show order history and details
- [ ] Demonstrate inventory reduction
- [ ] Show mobile checkout experience
- [ ] Review payment security measures
- [ ] Discuss any challenges faced
- [ ] Present Sprint 4 preview (admin panel & launch)

---

**Sprint 3 Start Date:** [To be determined]  
**Sprint 3 End Date:** [To be determined]  
**Sprint Review Date:** [To be determined]  
**Sprint Retrospective Date:** [To be determined]

---

## 🎉 Sprint 3 Success Criteria

At the end of Sprint 3, the Mamabear e-commerce platform should have a **fully functional transaction engine** that allows customers to:
1. Add products to their shopping cart
2. Proceed through a smooth checkout process
3. Select shipping methods with real-time cost calculation
4. Complete payment through multiple payment methods
5. Receive order confirmation via email
6. View their order history and track orders

This sprint transforms the platform from a product catalog into a **revenue-generating e-commerce system**! 🚀
