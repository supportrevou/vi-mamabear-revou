# 🐻 Project Brief: Mamabear E-commerce **Mobile-first** Web App

## 1. Project Overview

### 1.1 Executive Summary
* **Client:** Mamabear (UMKM)
* **Project Goal:** To build a **Mobile-first**, robust, scalable, and user-friendly e-commerce platform that allows Mamabear to transition from third-party marketplaces to a dedicated brand-owned channel.
* **Duration:** 8 Weeks (4 Sprints x 2 Weeks)
* **Methodology:** Agile-Lite (Direct implementation with bi-weekly Sprint Reviews)
* **Budget Consideration:** Cost-effective solution leveraging modern open-source technologies
* **Target Launch Date:** End of Sprint 4

### 1.2 Business Context
Mamabear is currently operating as a small-to-medium enterprise (UMKM) selling through third-party marketplaces. This project aims to:
* Establish direct customer relationships and brand identity
* Reduce dependency on marketplace fees and policies
* Gain full control over customer data and analytics
* Enable custom marketing and promotional strategies
* Build a foundation for future business scaling

---

## 2. Technical Strategy

### 2.1 Development Approach
This project follows a **code-first approach** by bypassing the formal UI/UX design phase. We will utilize a comprehensive **UI Component Library** (e.g., Tailwind UI, Shadcn/ui, or Mantine) to ensure professional aesthetics and rapid deployment.

### 2.2 Technology Stack

#### Front-End
* **Framework:** Next.js 14+ (React-based, with App Router)
* **Styling:** Tailwind CSS + UI Component Library (Shadcn/ui recommended)
* **State Management:** React Context API / Zustand for global state
* **Form Handling:** React Hook Form + validation
* **HTTP Client:** Axios or native Fetch API
* **Image Optimization:** Next.js Image component
* **SEO:** Next.js built-in SEO capabilities

#### Back-End
* **Runtime:** Node.js (LTS version)
* **Framework:** Express.js or NestJS
* **Database:** PostgreSQL
* **ORM:** Prisma or TypeORM
* **Authentication:** JWT + bcrypt for password hashing
* **File Storage:** Cloudinary or AWS S3
* **API Documentation:** Swagger/OpenAPI

#### Third-Party Integrations
* **Payment Gateway:** Midtrans or Xendit (Indonesian market)
* **Shipping:** RajaOngkir API for courier rates and tracking

#### DevOps & Infrastructure
* **Version Control:** Git + GitHub/GitLab
* **Hosting:** Vercel (FE) + Railway/Render/DigitalOcean (BE)
* **CI/CD:** GitHub Actions or GitLab CI

### 2.3 Architecture Overview
* **Pattern:** Monolithic backend with separate frontend (JAMstack approach)
* **API Design:** RESTful API with JSON responses
* **Authentication Flow:** JWT-based with refresh tokens
* **File Upload:** Direct upload to cloud storage with signed URLs

### 2.4 Team Structure & Responsibilities

| Role | Primary Responsibilities |
| :--- | :--- |
| **Front-End Developer** | UI implementation, state management, API consumption, responsive design, SEO optimization |
| **Back-End Developer** | Database architecture, business logic, API development, third-party integrations, security implementation |
| **Project Manager** | Sprint planning, stakeholder communication, timeline management, quality assurance |

---

## 3. Sprint Roadmap

### **Sprint 1: Foundation & The Visual Shell**
**Goal:** Establish the brand presence and secure access points.

| Role | Primary Tasks |
| :--- | :--- |
| **Front-End** | • Initialize project repository & setup UI Library.<br>• Develop **Homepage** layout using placeholder data.<br>• Implement Global Navigation (Navbar, Footer, Search bar UI). |
| **Back-End** | • Server & Database environment setup.<br>• Build **Authentication API** (Registration, Login, Password Reset).<br>• Build **Basic Product CRUD API** (Schema for names, prices, weight). |

### **Sprint 2: Discovery & Data Integration**
**Goal:** Transition from placeholders to real data and enable deep product browsing.

| Role | Primary Tasks |
| :--- | :--- |
| **Front-End** | • Develop **Product Detail Page (PDP)** (Galleries, variant selectors).<br>• Integrate **Auth API** (Enable real user login/session).<br>• Bind **Product Listing** to actual BE APIs (replacing placeholders). |
| **Back-End** | • Implement **Search & Filtering logic** (Category, price, keywords).<br>• Develop **Category Management API**.<br>• Setup **Media Storage** (Cloudinary/S3) for product images. |

### **Sprint 3: The Transaction Engine**
**Goal:** Convert visitors into customers via cart and checkout functionality.

| Role | Primary Tasks |
| :--- | :--- |
| **Front-End** | • Develop **Shopping Cart** functionality (Add/Edit/Delete).<br>• Develop **Checkout Flow UI** (Shipping forms & courier selection).<br>• Integrate Search/Filter UI with BE endpoints. |
| **Back-End** | • Integrate **Shipping API** (e.g., RajaOngkir) for real-time rates.<br>• Integrate **Payment Gateway** (e.g., Midtrans/Xendit).<br>• Develop **Order & Inventory Logic** (Invoices & stock reduction). |

### **Sprint 4: Management & Go-Live**
**Goal:** Provide administrative control and launch the production environment.

| Role | Primary Tasks |
| :--- | :--- |
| **Front-End** | • Develop **Admin Dashboard UI** (Order & Product management).<br>• Responsive UI Polishing (Mobile & Tablet optimization).<br>• Final Bug Fixing & SEO Tagging. |
| **Back-End** | • Implement **Admin Access Control** (RBAC).<br>• Develop **Sales Reporting API** (Daily/Monthly summaries).<br>• Production Deployment & Domain/SSL Configuration. |

---

## 4. Functional Requirements

### 4.1 Customer-Facing Features

#### User Authentication & Account Management
* User registration with email verification
* Login/logout functionality
* Password reset via email
* Profile management (name, email, phone, addresses)
* Order history viewing

#### Product Browsing & Discovery
* Homepage with featured products and categories
* Product listing pages with pagination
* Product detail pages with:
  * Multiple product images (gallery view)
  * Product variants (size, color, etc.)
  * Stock availability indicator
  * Product descriptions and specifications
* Search functionality with keyword matching
* Filtering by category, price range, and attributes
* Sorting options (price, newest, popularity)

#### Shopping Cart & Checkout
* Add/remove/update cart items
* Persistent cart (saved to database for logged-in users)
* Real-time stock validation
* Shipping address management (multiple addresses)
* Courier selection with real-time shipping cost calculation
* Order summary with itemized costs
* Multiple payment method options
* Order confirmation page and email

#### Order Management
* Order tracking and status updates
* Order cancellation (within allowed timeframe)
* Order invoice download/view

### 4.2 Admin Panel Features

#### Dashboard
* Sales overview (daily, weekly, monthly)
* Recent orders summary
* Low stock alerts
* Key performance metrics

#### Product Management
* Create/edit/delete products
* Bulk product upload (CSV import)
* Product variant management
* Image upload and management
* Category assignment
* Stock level management
* Product visibility toggle (publish/unpublish)

#### Order Management
* View all orders with filtering and search
* Order status updates (pending, processing, shipped, delivered, cancelled)
* Order details view
* Print packing slips/invoices
* Shipping label generation

#### Category Management
* Create/edit/delete categories
* Category hierarchy (parent/child categories)
* Category image upload

#### User Management
* View customer list
* View customer order history
* Admin user management (add/remove admin accounts)

#### Reports & Analytics
* Sales reports (by period, product, category)
* Customer analytics
* Inventory reports
* Export data to CSV/Excel

### 4.3 System Features

#### Security
* HTTPS encryption for all communications
* Password hashing with bcrypt
* JWT token-based authentication
* CSRF protection
* Rate limiting on API endpoints
* Input validation and sanitization
* SQL injection prevention
* XSS protection

#### Performance
* Image optimization and lazy loading
* API response caching
* Database query optimization
* CDN for static assets
* Gzip compression

#### SEO & Marketing
* SEO-friendly URLs
* Meta tags for products and pages
* Open Graph tags for social sharing
* Sitemap generation
* Google Analytics integration
* Structured data markup (Schema.org)

---

## 5. Documentation Deliverables

### 5.1 Technical Documentation
* **API Documentation:** Complete endpoint reference with examples
* **Database Schema:** ER diagrams and table descriptions
* **Architecture Documentation:** System design and data flow diagrams
* **Deployment Guide:** Step-by-step deployment instructions
* **Environment Configuration:** Required environment variables and settings

### 5.2 User Documentation
* **Admin User Guide:** How to manage products, orders, and settings
* **Customer FAQ:** Common questions and troubleshooting
* **Video Tutorials:** Screen recordings for key admin tasks

### 5.3 Operational Documentation
* **Maintenance Guide:** Routine maintenance tasks
* **Backup & Recovery:** Backup procedures and disaster recovery
* **Monitoring Guide:** How to use monitoring tools
* **Troubleshooting Guide:** Common issues and solutions
* **Security Best Practices:** Ongoing security recommendations

---

## 6. Communication & Collaboration

### 6.1 Meeting Schedule
* **Sprint Planning:** Start of each sprint (2 hours)
* **Daily Standups:** 15 minutes (async via Slack acceptable)
* **Sprint Review:** End of each sprint (1 hour)
* **Sprint Retrospective:** End of each sprint (30 minutes)
* **Ad-hoc Meetings:** As needed for blockers

### 6.2 Communication Channels
* **Project Management:** Jira, Trello, or Linear
* **Team Communication:** Slack or Discord
* **Code Repository:** GitHub or GitLab
* **Documentation:** Notion, Confluence, or Google Docs
* **Design Assets:** Figma (if needed for references)

### 6.3 Reporting
* **Weekly Status Report:** Progress, blockers, next steps
* **Sprint Review Demo:** Working features demonstration
* **Final Project Report:** Comprehensive project summary

---

## 7. Appendices

### Appendix A: Glossary
* **UMKM:** Usaha Mikro, Kecil, dan Menengah (Micro, Small, and Medium Enterprises)
* **JWT:** JSON Web Token
* **API:** Application Programming Interface
* **CRUD:** Create, Read, Update, Delete
* **SEO:** Search Engine Optimization
* **CDN:** Content Delivery Network
* **SSL:** Secure Sockets Layer
* **WCAG:** Web Content Accessibility Guidelines

### Appendix B: Reference Links
* Next.js Documentation: https://nextjs.org/docs
* Tailwind CSS: https://tailwindcss.com
* Shadcn/ui: https://ui.shadcn.com
* Midtrans: https://midtrans.com
* RajaOngkir: https://rajaongkir.com


---

## 8. Assessment Rubric

| Category | Criteria |
| :--- | :--- |
| **Business Logic Implementation** | The implemented feature(s) match the requirements and acceptance criteria of the chosen user story from the PRD. |
| | The application (Front End and Back End) is successfully deployed and accessible via valid links. |
| | The deployed functionality runs without critical bugs (does not crash during normal use). |
| **Documentation Craftsmanship** | Codebase is well-structured, maintainable, and follows best practices |
| | Commit messages follow the Conventional Commits format. |
| | Commits are made regularly and are micro-commits (small, focused changes). |
| **Team Integration** | Regular project updates and participation in team communications |
| | Effective use of conventional commits and active branching |
| **Issue Resolution** | Demonstrates effective debugging and error handling |
| | Handles edge cases and unexpected inputs gracefully |
| **Analytical Inquiry** | Requirements are properly translated into technical tasks |
| | Clearly documents key decisions and changes throughout the project |
