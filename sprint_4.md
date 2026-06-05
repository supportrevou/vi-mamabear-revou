# 🎯 Sprint 4: Management & Go-Live

## Sprint Overview

**Duration:** 2 Weeks  
**Sprint Goal:** Provide administrative control and launch the production environment  
**Status:** Planning  
**Sprint Period:** Week 7-8

---

## 🎯 Sprint Objectives

1. Build comprehensive admin dashboard for business management
2. Implement RBAC and admin access control
3. Develop sales reporting, customer management, and site settings
4. Polish UI for mobile/tablet and optimize SEO
5. Deploy to production and go live

---

## 👥 User Stories

### Admin User Stories

**US-4.1: View Dashboard Overview**  
As an admin, I want to see key business metrics at a glance, so that I can monitor store performance.  
**AC:** Dashboard shows total sales, orders, customers, products; sales chart (daily/weekly/monthly); recent orders; low stock alerts; top selling products.

---

**US-4.2: Manage Products (Advanced)**  
As an admin, I want to efficiently manage all products, so that I can keep the catalog up-to-date.  
**AC:** Product list with search, filter, sort, and bulk actions; create/edit form with image management and variants; CSV export; product duplication.

---

**US-4.3: Manage Orders (Advanced)**  
As an admin, I want to process and track orders efficiently, so that customers receive their orders on time.  
**AC:** Order list with search and filtering; order detail with tracking, invoice print, and status history; bulk status updates.

---

**US-4.4: Manage Categories**  
As an admin, I want to organize products into categories, so that customers can browse easily.  
**AC:** Hierarchical category tree view; create/edit form with image upload and SEO fields; activate/deactivate categories.

---

**US-4.5: View Sales Reports**  
As an admin, I want to view sales reports and analytics, so that I can make informed business decisions.  
**AC:** Sales report with date range, chart, and breakdown by category/product; top selling products; CSV export.

---

**US-4.6: Manage Customers**  
As an admin, I want to view customer information and manage accounts, so that I can provide better service.  
**AC:** Customer list with search, sort by total spent/total orders; customer detail with order history and saved addresses; block/reactivate accounts; CSV export.

---

**US-4.7: Control Admin Access**  
As a super admin, I want to manage admin user accounts, so that I can control who has access to the admin panel.  
**AC:** Create/deactivate admin users; assign roles (admin, super_admin); all admin actions logged; prevent self-role-change and deleting last super admin.

---

**US-4.8: Configure Site Settings**  
As an admin, I want to configure site-wide settings, so that I can customize the store without code changes.  
**AC:** Settings page with: site name, contact info, social links, shipping origin, tax rate, currency, maintenance mode.

---

### Customer User Stories

**US-4.9: Mobile Shopping Experience**  
As a mobile customer, I want a smooth shopping experience on my phone, so that I can shop conveniently anywhere.  
**AC:** All pages responsive on mobile; touch-friendly (min 44x44px targets); fast load times; tested on iOS and Android.

---

**US-4.10: Discover Store via Search Engines**  
As a potential customer, I want to find the store through search engines, so that I can discover products I'm looking for.  
**AC:** Meta tags, Open Graph, and Schema.org structured data on all pages; sitemap.xml and robots.txt configured; Google Analytics tracking purchases.

---

### Business User Stories

**US-4.15: Launch the Store**  
As a business owner, I want to launch the e-commerce store, so that I can start selling online.  
**AC:** All features tested; production deployed with domain, SSL, and monitoring active; payment gateway in production mode; admin trained on the system.

---

## 📋 Sprint Backlog

### 1. Admin Dashboard

**Goal:** Real-time overview of business performance.

**Tasks:**

- [FE] Build admin layout: sidebar navigation (Dashboard, Products, Orders, Categories, Customers, Reports, Users, Settings), collapsible sidebar on mobile, hamburger menu, responsive tables and forms
- [FE] Build dashboard overview page (`/admin/dashboard`): metrics cards (total sales, orders, customers, products), sales chart (daily/weekly/monthly), recent orders table, low stock alerts, top selling products
- [FE] Admin authentication: separate admin login page, role verification, redirect non-admin users, session management
- [BE] `GET /api/admin/reports/dashboard` — return: total revenue, order count, customer count, product count, recent orders, low stock products, top selling products
- [FE+BE] Connect dashboard UI with `GET /api/admin/reports/dashboard`

---

### 2. Product Management (Advanced)

**Goal:** Efficient full-lifecycle management of the product catalog.

**Tasks:**

- [FE] Build product list page (`/admin/products`): searchable table, filter by category/status/stock, sort by name/price/date, bulk actions (delete, publish/unpublish), pagination
- [FE] Build product create/edit form: name, description, SKU, price, stock, category, multiple image upload (drag-and-drop reorder), variants, SEO fields (meta title, meta description), publish/draft toggle
- [FE] Product image management: upload, preview, set featured image, delete, drag-and-drop reorder
- [FE] Product variant manager: add types (size, color), add values, set variant-specific price and stock
- [BE] Add `metaTitle`, `metaDescription` columns to Product schema via Prisma migration
- [BE] `POST /api/admin/products/bulk-delete` — bulk delete products
- [BE] `POST /api/admin/products/bulk-publish` — bulk publish/unpublish
- [BE] `GET /api/admin/products/export` — export to CSV (name, SKU, price, stock, category)
- [BE] `POST /api/admin/products/:id/duplicate` — duplicate product: reference existing image URLs (do not copy files in cloud storage), generate new SKU, mark as draft
- [FE+BE] Connect product list UI with bulk operations and export endpoints

---

### 3. Order Management (Advanced)

**Goal:** Efficient order processing with tracking, invoicing, and status history.

**Tasks:**

- [FE] Build order list page (`/admin/orders`): searchable table, filter by status/date/payment, sort, status badges, quick status update, CSV export, pagination
- [FE] Build order detail page (`/admin/orders/[id]`): order info, customer info, items, shipping address, payment info, order timeline/history, status update dropdown, add tracking number form, print invoice button, cancel order button
- [FE] Order status management: status update modal with notes, bulk status update
- [FE] Printable invoice template: layout, company branding, order details, print-friendly CSS
- [BE] `PUT /api/admin/orders/:id/status` — update order status, validate transitions, add notes, log to order history (no email notifications)
- [BE] `PUT /api/admin/orders/:id/tracking` — add/update tracking number; displayed on customer's order page
- [BE] `GET /api/admin/orders/:id/invoice` — return structured order data for browser-based print rendering; invoice numbering system
- [BE] Order search and filtering: by order number, customer name/email, status, date range, payment status; pagination
- [BE] `GET /api/admin/orders/export` — export orders to CSV
- [FE+BE] Connect order UI with status update, tracking, invoice, and export endpoints

---

### 4. Category Management

**Goal:** Organized category hierarchy with SEO support.

**Tasks:**

- [FE] Build category list page (`/admin/categories`): hierarchical tree view (expand/collapse, indentation for subcategories), delete category, "Add New Category" button
- [FE] Build category create/edit form: name, slug, parent category, description, image upload, SEO fields (meta title, meta description), display order, active/inactive toggle
- [BE] Add `metaTitle`, `metaDescription` columns to Category schema via Prisma migration
- [FE+BE] Connect category UI with Sprint 2 category endpoints (`GET /api/categories`, `POST /api/categories`, `PUT /api/categories/:id`, `DELETE /api/categories/:id`)

---

### 5. Customer Management

**Goal:** Admin visibility into customer behavior and account control.

**Tasks:**

- [FE] Build customer list page (`/admin/customers`): search, filter by registration date, sort by name/email/total spent/total orders, CSV export
- [FE] Build customer detail page (`/admin/customers/[id]`): profile info, order history, total spent, average order value, last order date, registered addresses, account status (active/blocked)
- [BE] `GET /api/admin/customers` — list all users with role=customer; query params: `search`, `sort`, `order`, `page`, `limit`; response: id, name, email, phone, total_orders, total_spent, registered_at
- [BE] `GET /api/admin/customers/:id` — customer detail: profile, saved addresses, order history summary, total_spent, average_order_value, last_order_date
- [BE] Aggregation logic: calculate total_orders, total_spent, average_order_value per customer via DB aggregation
- [BE] `GET /api/admin/customers/export` — export to CSV
- [BE] `PUT /api/admin/customers/:id/status` — block or reactivate; blocked customers cannot log in
- [FE+BE] Connect customer list with `GET /api/admin/customers` (search, sort, filter, pagination)
- [FE+BE] Connect customer detail with `GET /api/admin/customers/:id`, including saved addresses from Sprint 3 Address API (`GET /api/users/addresses`)
- [FE+BE] Connect block/unblock button with `PUT /api/admin/customers/:id/status`

---

### 6. Sales Reports & Analytics

**Goal:** Data-driven sales insights by period, category, and product.

**Tasks:**

- [FE] Build sales report page (`/admin/reports/sales`): date range selector, summary (total sales, orders, avg order value), line/bar chart, sales by category, sales by product, CSV export
- [FE] Build product performance report: top selling products, lowest-selling products (fewest sales)
- [FE] Data visualization: Chart.js or Recharts (interactive, responsive charts)
- [BE] `GET /api/admin/reports/sales` — aggregated sales by date range (daily/weekly/monthly), by category, by product
- [BE] `GET /api/admin/reports/products` — product performance: sales count and revenue per product/category
- [BE] Analytics logic: total revenue, order count, avg order value, revenue trends, top selling products
- [BE] Export: CSV with Content-Disposition header
- [BE] Filter support: date range, category, product, status
- [FE+BE] Connect reports UI with `GET /api/admin/reports/sales` and `GET /api/admin/reports/products`

---

### 7. Admin Access Control (RBAC)

**Goal:** Enforce role-based access and allow super admins to manage admin accounts.

**Tasks:**

- [BE] Add role field (customer, admin, super_admin) to User schema via Prisma migration
- [BE] Admin authentication middleware: verify JWT, check role (admin/super_admin only for admin routes), restrict access
- [BE] Role-based permissions: admin manages products/orders/categories; super_admin manages admin users; customer accesses customer features only
- [BE] Admin user management endpoints:
  - `GET /api/admin/users` — list admin/staff users (not customers)
  - `GET /api/admin/users/:id` — get admin user detail
  - `POST /api/admin/users` — create admin user (super_admin only)
  - `PUT /api/admin/users/:id/role` — update role (super_admin only)
  - `PUT /api/admin/users/:id/status` — deactivate or reactivate admin user (super_admin only)
  - `DELETE /api/admin/users/:id` — permanently delete user (super_admin only)
- [BE] Validation: prevent self-deactivation, prevent deactivating last super_admin
- [BE] `AdminActivityLog` schema (id, userId, action, entity, entityId, createdAt) + Prisma migration; log all admin actions with timestamp
- [FE] Build admin user management page (`/admin/users`): list admin users, create, deactivate, assign roles
- [FE+BE] Verify RBAC: admin cannot access super-admin routes, customers cannot access admin routes

---

### 8. Site Settings

**Goal:** Configurable site-wide settings without code changes.

**Tasks:**

- [BE] Settings schema (id, key, value, type, description) + Prisma migration
- [BE] Settings endpoints: `GET /api/admin/settings`, `GET /api/admin/settings/:key`, `PUT /api/admin/settings/:key`; validate types, sanitize input
- [BE] Configurable settings: site name, description, contact info, social links, shipping origin, tax rate, currency, maintenance mode toggle
- [FE] Build settings page (`/admin/settings`): form grouped by section, save confirmation
- [FE+BE] Connect settings UI with settings API

---

### 9. Responsive UI Polishing

**Goal:** Smooth, touch-friendly experience on mobile and tablet across all pages.

**Tasks:**

- [FE] Audit all pages for responsive issues: homepage, product listing, product detail, cart, checkout, account pages, admin pages
- [FE] Fix responsive issues: layout breaks, overlapping elements, inaccessible buttons, unreadable text
- [FE] Touch optimization: tap targets min 44x44px, mobile-friendly forms
- [FE] Test on 3 representative screen sizes: mobile (375px), tablet (768px), desktop (1280px)
- [FE] Test on real devices: iOS (iPhone), Android (Samsung or equivalent popular device)

---

### 10. SEO Optimization

**Goal:** Make store pages discoverable via search engines.

**Tasks:**

- [FE] SEO meta tags: unique title and meta description per page, Open Graph tags for social sharing, canonical URLs
- [FE] Dynamic product meta tags: product name in title, description in meta, product image in OG tags
- [FE] Structured data (Schema.org): Product schema, Breadcrumb schema
- [FE] Generate sitemap.xml (all public pages); submit to Google Search Console after production deploy
- [FE] Create robots.txt: allow/disallow paths, link to sitemap
- [FE] Page speed optimization: image lazy loading, code splitting, minimize bundle size, optimize fonts
- [FE] Google Analytics 4: track page views and purchase event (order confirmation page)

---

### 11. Final QA & Bug Fixing

**Goal:** Ship a stable, tested application to production.

**Tasks:**

- [FE] Functional testing: all features end-to-end
- [FE] Cross-browser testing: Chrome, Firefox, Safari
- [FE] Basic accessibility check: color contrast on key UI elements, form labels and input associations
- [FE] Performance optimization: Lighthouse audit, fix issues, optimize images, minimize JS
- [FE+BE] End-to-end testing on staging environment
- [FE+BE] UAT with client (focus on admin dashboard, order flow, customer listing)
- [FE+BE] Bug fixing: prioritize by severity (P0, P1, P2), document known issues

---

### 12. Production Deployment

**Goal:** Launch the store to a live, monitored production environment.

**Tasks:**

- [BE] Set up production PostgreSQL: create instance, configure credentials, run migrations, seed initial data (admin user, categories)
- [BE] Configure production environment variables: DB connection string, JWT secret, API keys (payment, shipping, cloud storage), CORS origins, Node environment
- [BE] Deploy to cloud platform (Railway, Render, or DigitalOcean): configure resources, Node.js runtime (process management handled by platform)
- [BE] Production logging: Winston logger, log to external service only (Logtail, Papertrail, or Sentry), error tracking via Sentry
- [BE] Monitoring: server health, DB, and API endpoint monitoring; alerts for critical issues
- [BE] Backup strategy: automated DB backups, backup retention policy, test restoration
- [BE] Rate limiting: stricter production limits, IP-based, DDoS protection
- [BE] Security hardening: audit (auth, authorization, input validation, SQL injection, XSS, CORS), security headers (CSP, X-Frame-Options, X-Content-Type-Options, HSTS), HTTPS enforcement, request validation, security monitoring; document security practices applied
- [FE] Deploy to Vercel: connect GitHub repo, configure build settings, set environment variables, configure custom domain
- [FE] Configure production API endpoints: update base URL, configure CORS, test connectivity
- [FE] SSL: verify SSL is active and HTTPS redirect works on custom domain (Vercel handles SSL automatically)
- [FE] Domain: configure DNS records, point to Vercel, configure www redirect
- [FE+BE] Verify checkout flow uses Sprint 3 Address API (`GET /api/users/addresses`) for saved address selection
- [FE+BE] Smoke test on production: all critical paths (register, browse, checkout, payment, admin login, order management)

---

## 📊 Definition of Done

### For Each Feature

- [ ] FE and BE tasks completed and tested together
- [ ] Code reviewed by at least one team member
- [ ] No P0 or P1 bugs
- [ ] Code merged to main branch

### For Sprint 4 Completion

- [ ] All admin features working on staging
- [ ] Application responsive on mobile and tablet
- [ ] SEO meta tags and structured data in place
- [ ] All P0 and P1 bugs fixed
- [ ] Deployed to production with domain and SSL active
- [ ] Monitoring and logging active
- [ ] API documentation updated for all Sprint 4 endpoints
- [ ] Client training completed
- [ ] Pre-launch checks done: HTTPS active, payment gateway in production mode, admin account created, initial products and categories seeded

---

## 🎨 Design Guidelines

### Admin Dashboard

- **Layout:** Sidebar navigation with main content area
- **Color Scheme:** Professional, distinct from customer-facing site
- **Typography:** Clear, readable fonts for data-heavy interfaces
- **Data Visualization:** Chart.js or Recharts (interactive, responsive)
- **Tables:** Sortable, filterable, with clear actions
- **Forms:** Well-organized, with clear labels and validation

### Responsive Design

- **Mobile First:** Design for mobile, enhance for desktop
- **Breakpoints:** 375px (mobile), 768px (tablet), 1280px (desktop)
- **Touch Targets:** Minimum 44x44px for mobile
- **Navigation:** Hamburger menu on mobile, full sidebar on desktop

---

## 🔐 Security Considerations

- **Authentication:** Strong passwords required for admin accounts; short session timeout
- **Authorization:** Strict RBAC enforcement; all admin endpoints role-checked
- **Activity Logging:** All admin actions logged to `AdminActivityLog`
- **Environment Variables:** Never commit secrets to repository
- **HTTPS:** Force HTTPS in production; configure HSTS
- **Security Headers:** CSP, X-Frame-Options, X-Content-Type-Options
- **Rate Limiting:** Brute force and DDoS protection on all endpoints
- **Input Validation:** Validate and sanitize all inputs; use parameterized queries
- **Error Handling:** Do not leak sensitive information in error responses
