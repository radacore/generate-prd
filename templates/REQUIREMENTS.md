# REQUIREMENTS.md: ProfilKorp

## 1. Functional Requirements

### 1.1. Public-Facing Application

**FR-01: Homepage Display**
The system SHALL display a dynamic homepage that includes a hero banner, featured services, latest blog posts, and client testimonials.
*   The homepage MUST load with all specified sections visible.
*   Content within each section (e.g., featured services, blog posts) SHALL dynamically update based on data managed in the Admin Dashboard.
*   Clicking on featured items (e.g., a service, a blog post) SHOULD navigate to its respective detail page.

**FR-02: About Us Page Content Management**
The system SHALL provide an "About Us" page displaying company history, mission, vision, and core values. All content on this page MUST be fully editable via the Admin Dashboard.
*   The "About Us" page MUST display the content exactly as entered and saved by the administrator.
*   Changes made to the "About Us" content in the Admin Dashboard SHALL be immediately reflected on the public page upon saving.
*   The page SHOULD include sections for history, mission, vision, and values, each populated with editable text.

**FR-03: Services Listing and Detail Pages**
The system SHALL display a list of all company services. Each service MUST have its own dedicated detail page, including descriptions, images, and related case studies.
*   The services listing page MUST show all active services with a title and a brief description.
*   Clicking on a service from the listing page SHALL navigate to its unique detail page.
*   Each service detail page MUST display its full description, associated images, and links to related case studies (if any).

**FR-04: Portfolio/Projects Gallery**
The system SHALL feature a filterable gallery of completed projects. Each project entry MUST include a title, description, image/video gallery, and associated service category.
*   The portfolio page MUST display projects in a gallery format, allowing filtering by category.
*   Each project entry SHALL link to a detail page showcasing its title, full description, and a gallery of associated media (images/videos).
*   The filtering mechanism MUST accurately display projects belonging to the selected category.

**FR-05: Team Member Profiles**
The system SHALL display profiles of key team members, including their name, title, photo, and a brief biography.
*   The team page MUST list all active team members with their photo, name, and title.
*   Clicking on a team member's profile SHOULD reveal their brief biography.
*   Team member information SHALL be dynamically sourced from the Admin Dashboard.

**FR-06: Blog/News Section**
The system SHALL include a blog/news section displaying articles in reverse-chronological order, with pagination for the list view and a dedicated detail view for individual articles. It MUST support categories and tags.
*   The blog listing page MUST display articles with pagination, showing the most recent first.
*   Each article detail page MUST display the full content, author, publication date, and associated categories/tags.
*   Users SHOULD be able to filter blog posts by category or tag.

**FR-07: Testimonials Display**
The system SHALL display a rotating or static list of client testimonials, including the client's name and company.
*   The testimonials section MUST display only testimonials marked as "approved" in the Admin Dashboard.
*   Each testimonial displayed MUST include the client's name and company.
*   The display SHOULD support either a rotating carousel or a static list format.

**FR-08: Contact Us Page and Form**
The system SHALL provide a "Contact Us" page with company contact details (address, phone, email), an interactive map, and a contact form. Submitting the form MUST send an email notification to a predefined admin address and store the submission in the database.
*   The "Contact Us" page MUST display the company's address, phone number, and email address.
*   The contact form MUST include fields for Name, Email, Subject, and Message.
*   Upon successful submission, the system SHALL send an email notification to the configured admin email address and store the submission details in the database.

**FR-09: SEO & Social Sharing Optimization**
All public pages MUST have unique, human-readable URLs. Meta titles, descriptions, and Open Graph tags MUST be dynamically generated based on content and be manageable from the Admin Dashboard.
*   Each public page SHALL have a clean, SEO-friendly URL (e.g., `/services/service-name`).
*   The `meta title`, `meta description`, and Open Graph tags (e.g., `og:title`, `og:description`, `og:image`) for each page MUST be present in the HTML header.
*   Changes to SEO metadata in the Admin Dashboard SHALL be reflected in the public page's HTML source.

### 1.2. Admin Dashboard

**FR-10: Secure Administrator Authentication**
The system SHALL provide a dedicated login page for a single administrator account, implementing session-based authentication via Laravel Sanctum.
*   The administrator MUST be able to log in successfully using valid credentials.
*   Invalid login attempts SHALL result in an error message and prevent access.
*   The system MUST maintain the administrator's session across page navigations until explicit logout or session expiration.

**FR-11: Main Dashboard Overview**
The system SHALL present a main dashboard overview page displaying integrated analytics (e.g., Google Analytics widgets), a summary of recent contact form submissions, and quick links to content management sections.
*   The dashboard MUST display a summary of key metrics (e.g., visitor count, top pages) from the integrated analytics service.
*   A section on the dashboard MUST list the most recent contact form submissions.
*   Quick links to content management areas (e.g., "Add New Blog Post", "Manage Services") MUST be present and functional.

**FR-12: Page Content Management (WYSIWYG)**
The system SHALL provide a WYSIWYG editor (e.g., TinyMCE) within the Admin Dashboard to manage the content of static pages such as "About Us" and "Services".
*   The WYSIWYG editor MUST allow administrators to format text, insert images, and create links for page content.
*   Content saved through the WYSIWYG editor MUST be accurately displayed on the corresponding public page.
*   The editor SHOULD support common rich text functionalities like bold, italic, lists, and headings.

**FR-13: Portfolio Project Management (CRUD)**
The system SHALL provide a full CRUD (Create, Read, Update, Delete) interface for managing portfolio projects. This includes fields for title, description, client, date, and an interface to upload/manage associated images and videos.
*   The administrator MUST be able to create new portfolio projects with all required details.
*   Existing portfolio projects MUST be editable and deletable.
*   The interface MUST allow uploading multiple images and videos for each project and associating them correctly.

**FR-14: Team Member Management (CRUD)**
The system SHALL provide a full CRUD interface for managing team member profiles.
*   The administrator MUST be able to add, edit, and delete team member profiles.
*   Each team member profile MUST include fields for name, title, photo, and a brief biography.
*   Changes made to team member profiles SHALL be reflected on the public team page.

**FR-15: Blog Post Management (CRUD)**
The system SHALL provide a full CRUD interface for managing blog posts, including managing categories, tags, author, and publication status (Draft, Published).
*   The administrator MUST be able to create, edit, and delete blog posts.
*   Each blog post MUST support assignment to one or more categories and tags.
*   The administrator SHALL be able to set a blog post's status to "Draft" or "Published", controlling its visibility on the public site.

**FR-16: Testimonial Management (CRUD)**
The system SHALL provide a full CRUD interface for managing testimonials, including an "approved" status to control visibility on the public site.
*   The administrator MUST be able to add, edit, and delete client testimonials.
*   Each testimonial MUST have a toggle or checkbox to set its "approved" status.
*   Only testimonials marked as "approved" SHALL be displayed on the public website.

**FR-17: Contact Submission Viewer**
The system SHALL provide a view within the Admin Dashboard to list, read, and archive all messages submitted through the public contact form.
*   The administrator MUST be able to view a list of all contact form submissions.
*   Clicking on a submission from the list SHALL display its full details (Name, Email, Subject, Message, Submission Date).
*   The administrator SHALL be able to mark submissions as "archived" to remove them from the active list.

**FR-18: Centralized Media Library**
The system SHALL provide a centralized interface for managing all uploaded media. All files MUST be stored on and served from AWS S3. The library MUST support image uploads, deletions, and browsing.
*   The administrator MUST be able to upload images and other media files through the interface.
*   Uploaded files MUST be stored in the configured AWS S3 bucket.
*   The media library MUST display a browsable list of all uploaded media, allowing for selection and deletion.

**FR-19: Advanced SEO Management**
The system SHALL provide a dedicated section within each page/post editor to manually override meta titles, meta descriptions, and Open Graph tags.
*   For each editable content item (e.g., page, blog post, service), the administrator MUST have fields to input custom meta title, meta description, and Open Graph properties.
*   Custom SEO values entered SHALL override any dynamically generated values for that specific content item.
*   The system SHALL validate the length of meta titles and descriptions to adhere to common SEO best practices.

**FR-20: Content Versioning**
For critical pages (e.g., About Us, Service details), the system MUST save a history of content changes. The administrator MUST be able to view past versions and rollback to a previous version.
*   The system SHALL automatically create a new version record whenever critical page content is saved.
*   The administrator MUST be able to view a list of historical versions for a given page, including the date and user who made the change.
*   The administrator SHALL be able to select a previous version and restore it as the current active content.

**FR-21: Global Settings Management**
The system SHALL provide an interface to manage site-wide information: Company Name, Logo, Favicon, Contact Email, Phone Number, Address, and Social Media Profile links.
*   The administrator MUST be able to update global settings such as company name, contact details, and social media links.
*   The interface MUST allow uploading and updating the company logo and favicon.
*   Changes to global settings SHALL be immediately reflected across the public website where applicable.

## 2. Non-Functional Requirements

| Category | Requirement | Measurable Target |
|:---|:---|:---|
| Performance | Fast Page Loads | LCP < 2.5s; FCP < 1.8s; TTFB < 200ms for cached pages |
| Security | System Integrity | HTTPS enforced; Protection against OWASP Top 10 (XSS, CSRF, SQLi); Admin URL obfuscated (not `/admin`); Rate limiting on login and contact form endpoints. |
| Scalability | Traffic Handling | Application layer stateless; Handle 1,000 concurrent users on public pages with < 500ms response time. |
| Usability | Accessibility & UX | Fully responsive design (mobile, tablet, desktop); Adherence to WCAG 2.1 Level AA; Corporate, professional, and clean UI. |
| Maintainability | Code Quality | Adherence to PSR-12 for PHP; High test coverage (>80%) for backend; Well-documented code and components. |
| Reliability | System Uptime | Target 99.9% uptime for public application; All media uploads transactional and resilient to failure. |

## 3. Technical Constraints

*   The system MUST be built for a single company only; multi-tenancy is explicitly not required.
*   The system MUST support only one administrator account; a role-based access control (RBAC) system is not to be implemented.
*   The technology stack (Laravel 12, React, Inertia.js, AWS S3) is fixed and non-negotiable.
*   The admin dashboard URL MUST be obfuscated and not use common paths like `/admin` or `/dashboard`.

## 4. Assumptions

*   The client WILL provide all initial content (text, images, branding guidelines) before the development freeze.
*   The client WILL provide credentials for a Google Analytics account for dashboard integration.
*   The administrator user IS technically proficient enough to use a WYSIWYG editor and manage digital content.
*   The hosting environment (VPS) WILL be configured with necessary server software (Nginx/Apache, PHP, MySQL) and sufficient resources.
*   AWS S3 bucket and IAM user credentials WILL be provided for media storage.