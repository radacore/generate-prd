# PRD: ProfilKorp

## Executive Summary & Product Vision

This document outlines the Product Requirements for **ProfilKorp**, a modern, single-company profile application with an advanced administrative dashboard.

The vision is to provide a dynamic, high-performance, and secure public-facing corporate website where all content is easily manageable through a comprehensive admin panel. The system will leverage a modern monolithic architecture using Laravel and React with Inertia.js to deliver a seamless user experience for both public visitors and the administrator.

## Problem Statement & Target Users

**Problem:** Traditional static company websites are difficult and time-consuming for non-technical staff to update. This leads to outdated information, poor user engagement, and missed business opportunities. Existing CMS solutions can be overly complex or lack specific features needed for a corporate profile.

**Target Users:**
*   **Public Visitors:** Potential customers, business partners, investors, and job applicants seeking information about the company.
*   **Administrator:** A single, designated internal user (e.g., Marketing Manager, IT staff) responsible for managing all website content, media, and settings.

## System Scope & User Roles

The system is composed of two primary components: a public-facing website and a private, secure admin dashboard. The scope is limited to a single company profile and a single administrator account.

| Role | Description | Permissions |
|:---|:---|:---|
| **Public User** (Anonymous) | Any visitor to the public website. | - View all public pages (Home, About, Services, etc.). - Submit the contact form. |
| **Administrator** | The single, authenticated user managing the site. | - Full CRUD access to all site content. - Manage media uploads to cloud storage. - View contact form submissions. - Configure site-wide settings and SEO. - Access analytics dashboard. |

## Functional Requirements

### Public-Facing Application
*   **FR-01: Homepage:** Display a summary of key sections: hero banner, featured services, latest blog posts, and client testimonials.
*   **FR-02: About Us Page:** Present company history, mission, vision, and core values. Content is fully editable from the admin panel.
*   **FR-03: Services Page:** List all company services. Each service must have its own detail page with descriptions, images, and related case studies.
*   **FR-04: Portfolio/Projects Page:** A filterable gallery of completed projects. Each project includes a title, description, image/video gallery, and associated service category.
*   **FR-05: Team Page:** Display profiles of key team members, including name, title, photo, and a brief bio.
*   **FR-06: Blog/News Section:** A reverse-chronological listing of articles. Includes a list view with pagination and a detail view for individual articles. Supports categories/tags.
*   **FR-07: Testimonials Section:** Display a rotating or static list of client testimonials with client name and company.
*   **FR-08: Contact Us Page:** Provide company contact details (address, phone, email) and an interactive map. Includes a contact form (Name, Email, Subject, Message) that sends an email notification to a predefined admin address and stores the submission in the database.
*   **FR-09: SEO & Social Sharing:** All public pages must have unique, human-readable URLs. Meta titles, descriptions, and Open Graph tags must be dynamically generated based on content and be manageable from the admin panel.

### Admin Dashboard
*   **FR-10: Secure Authentication:** A dedicated login page for a single administrator account. Implements session-based authentication via Laravel Sanctum.
*   **FR-11: Main Dashboard:** An overview page displaying integrated analytics (e.g., Google Analytics widgets for visitors, top pages), a summary of recent contact form submissions, and quick links to content management sections.
*   **FR-12: Page Content Management:** A WYSIWYG editor (e.g., TinyMCE) to manage the content of static pages like "About Us" and "Services".
*   **FR-13: Portfolio Management:** Full CRUD interface for portfolio projects. Includes fields for title, description, client, date, and an interface to upload/manage associated images and videos.
*   **FR-14: Team Management:** Full CRUD interface for team member profiles.
*   **FR-15: Blog Management:** Full CRUD interface for blog posts, including managing categories, tags, author, and publication status (Draft, Published).
*   **FR-16: Testimonial Management:** Full CRUD interface for testimonials, including an "approved" status to control visibility on the public site.
*   **FR-17: Contact Submission Viewer:** A view to list, read, and archive all messages submitted through the public contact form.
*   **FR-18: Media Library:** Centralized interface for managing all uploaded media. All files must be stored on and served from AWS S3. Supports image uploads, deletions, and browsing.
*   **FR-19: Advanced SEO Management:** A dedicated section within each page/post editor to manually override meta titles, meta descriptions, and Open Graph tags.
*   **FR-20: Content Versioning:** For critical pages (e.g., About Us, Service details), the system must save a history of content changes. The administrator must be able to view past versions and rollback to a previous version.
*   **FR-21: Global Settings Management:** An interface to manage site-wide information: Company Name, Logo, Favicon, Contact Email, Phone Number, Address, and Social Media Profile links.

## Non-Functional Requirements

| Category | Requirement | Metric / Target |
|:---|:---|:---|
| **Performance** | Fast Page Loads | - Largest Contentful Paint (LCP) < 2.5s - First Contentful Paint (FCP) < 1.8s - TTFB < 200ms for cached pages |
| **Security** | System Integrity | - HTTPS enforced on all pages. - Protection against OWASP Top 10 (XSS, CSRF, SQLi). - Admin URL is obfuscated (not `/admin`). - Rate limiting on login and contact form endpoints. |
| **Scalability** | Traffic Handling | - Application layer must be stateless to support horizontal scaling. - Handle 1,000 concurrent users on public pages with < 500ms response time. |
| **Usability** | Accessibility & UX | - Fully responsive design (mobile, tablet, desktop). - Adherence to WCAG 2.1 Level AA standards. - Corporate, professional, and clean UI. |
| **Maintainability** | Code Quality | - Adherence to PSR-12 coding standards for PHP. - High test coverage (>80%) for the backend. - Well-documented code and components. |
| **Reliability** | System Uptime | - Target 99.9% uptime for the public-facing application. - All media uploads must be transactional and resilient to failure. |

## Technology Stack & Rationale

| Component | Technology | Rationale (Why) |
|:---|:---|:---|
| Backend | Laravel 12 | User-specified. A robust, mature PHP framework with a vast ecosystem perfect for rapid development. |
| Frontend | React 18+ | User-specified. A powerful library for building interactive UIs, providing a rich user experience. |
| Adapter | Inertia.js | User-specified. Bridges Laravel and React, enabling a modern SPA-like feel without the complexity of a separate API. |
| Styling | Tailwind CSS | A utility-first CSS framework that enables rapid, custom UI development suitable for a corporate design aesthetic. |
| Database | MySQL 8+ | Industry-standard, reliable, and fully supported by Laravel. Excellent performance for this application's scale. |
| File Storage | AWS S3 | User-specified. A highly scalable, durable, and cost-effective cloud storage solution for media assets. |
| Authentication | Laravel Sanctum | Lightweight, designed for SPAs and mobile apps. Perfect for session-based auth with Inertia.js. |
| Build Tool | Vite | Default with Laravel 12. Provides extremely fast Hot Module Replacement (HMR) and optimized production builds. |
| Hosting | VPS (e.g., Linode) | Provides full control over the server environment, cost-effective, and sufficient for the project's needs. |

## Success Metrics & KPIs

| Metric | KPI | Target |
|:---|:---|:---|
| User Engagement | Average Time on Page | > 90 seconds on key pages (Services, Portfolio). |
| Lead Generation | Contact Form Submissions | ≥ 10 valid submissions per month after launch. |
| Admin Efficiency | Content Update Time | < 5 minutes for a standard content update (e.g., new blog post). |
| Technical Performance | Google PageSpeed Insights | Score > 90 for Mobile on the homepage. |
| Brand Reach | Organic Search Traffic | 20% increase in organic traffic within 6 months post-launch. |

## Risk Analysis & Mitigation

| Risk | Impact | Mitigation Strategy |
|:---|:---|:---|
| **Admin Panel Breach** | High | Unauthorized content changes, data theft. | - Enforce strong password policy. - Implement 2-Factor Authentication (2FA). - Use IP whitelisting for admin access if possible. - Implement strict rate limiting on the login endpoint. |
| **Data Loss on S3** | High | Loss of all company media assets. | - Enable versioning on the AWS S3 bucket. - Implement a restrictive IAM policy for the application user. - Regularly back up bucket metadata. |
| **Performance Bottlenecks** | Medium | Poor user experience, high bounce rate. | - Implement aggressive caching strategies (page, query, config) using Redis. - Use a Content Delivery Network (CDN) for assets. - Optimize database queries and use eager loading. |
| **Vendor Lock-in (AWS)** | Low | Increased costs or difficulty migrating. | - Use Laravel's abstract `Filesystem` driver. This allows switching the storage provider (e.g., to DigitalOcean Spaces) with minimal code changes. |

## Constraints & Assumptions

*   **Constraint:** The system will be built for a single company only. Multi-tenancy is not required.
*   **Constraint:** The system will support only one administrator account. A role-based access control (RBAC) system is not needed.
*   **Constraint:** The technology stack (Laravel 12, React, Inertia, AWS S3) is fixed and non-negotiable.
*   **Assumption:** The client will provide all initial content (text, images, branding guidelines) before the development freeze.
*   **Assumption:** The client will provide credentials for a Google Analytics account for dashboard integration.
*   **Assumption:** The administrator user is technically proficient enough to use a WYSIWYG editor and manage digital content.

## Out of Scope

The initial release of ProfilKorp will **NOT** include:
*   Multi-language support.
*   E-commerce functionality (product sales, payment gateways).
*   Public user registration, profiles, or comments.
*   A multi-role permission system for the admin dashboard.
*   Automated CI/CD pipeline setup and configuration.
*   An integrated email marketing or newsletter system.