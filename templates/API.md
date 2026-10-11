# API.md: ProfilKorp

## Authentication & Authorization

ProfilKorp uses **Laravel Sanctum** with session-based authentication for the admin dashboard. Public endpoints require no authentication. Admin endpoints require a valid authenticated session.

**Session Header Format:**
```
Cookie: XSRF-TOKEN=<token>; laravel_session=<session_id>
X-CSRF-TOKEN: <token>
```

**Authorization Levels:**
- **Public:** No authentication required. Accessible to all visitors.
- **Admin:** Requires authenticated session. Only the single administrator can access.

## Standard Response & Pagination Formats

**Success Response (200 OK):**
```json
{
  "success": true,
  "data": { /* resource or array of resources */ },
  "message": "Operation completed successfully"
}
```

**Error Response (4xx/5xx):**
```json
{
  "success": false,
  "error": "Error code or message",
  "message": "Human-readable error description"
}
```

**Pagination Format (for list endpoints):**
```json
{
  "success": true,
  "data": [ /* array of items */ ],
  "pagination": {
    "current_page": 1,
    "per_page": 15,
    "total": 120,
    "last_page": 8,
    "from": 1,
    "to": 15
  }
}
```

## Public API Endpoints

### Homepage & General Content

#### Get Homepage Data
- **Method:** `GET`
- **Path:** `/api/public/homepage`
- **Description:** Retrieve all data for the homepage including hero banner, featured services, latest blog posts, and testimonials.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "hero": {
      "title": "string",
      "subtitle": "string",
      "image_url": "string",
      "cta_text": "string",
      "cta_link": "string"
    },
    "featured_services": [
      {
        "id": "integer",
        "name": "string",
        "description": "string",
        "icon_url": "string"
      }
    ],
    "latest_posts": [
      {
        "id": "integer",
        "title": "string",
        "excerpt": "string",
        "published_at": "datetime",
        "slug": "string"
      }
    ],
    "testimonials": [
      {
        "id": "integer",
        "content": "string",
        "client_name": "string",
        "client_company": "string",
        "image_url": "string"
      }
    ]
  }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

#### Get Global Settings
- **Method:** `GET`
- **Path:** `/api/public/settings`
- **Description:** Retrieve site-wide settings including company name, logo, contact info, and social media links.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "company_name": "string",
    "logo_url": "string",
    "favicon_url": "string",
    "contact_email": "string",
    "contact_phone": "string",
    "address": "string",
    "social_media": {
      "facebook": "string",
      "twitter": "string",
      "linkedin": "string",
      "instagram": "string"
    }
  }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

### About Us Page

#### Get About Us Content
- **Method:** `GET`
- **Path:** `/api/public/about`
- **Description:** Retrieve the About Us page content including company history, mission, vision, and core values.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "content": "string (HTML)",
    "mission": "string",
    "vision": "string",
    "values": [
      {
        "title": "string",
        "description": "string"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 500 (Server Error)

### Services

#### List All Services
- **Method:** `GET`
- **Path:** `/api/public/services`
- **Description:** Retrieve a paginated list of all company services.
- **Auth Level:** Public
- **Query Parameters:** `page=1`, `per_page=10`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "slug": "string",
      "description": "string",
      "icon_url": "string",
      "image_url": "string"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

#### Get Service Detail
- **Method:** `GET`
- **Path:** `/api/public/services/{slug}`
- **Description:** Retrieve detailed information for a specific service including description, images, and related case studies.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "slug": "string",
    "description": "string (HTML)",
    "icon_url": "string",
    "image_url": "string",
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "related_projects": [
      {
        "id": "integer",
        "title": "string",
        "slug": "string",
        "image_url": "string"
      }
    ]
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 500 (Server Error)

### Portfolio / Projects

#### List All Projects
- **Method:** `GET`
- **Path:** `/api/public/projects`
- **Description:** Retrieve a paginated, filterable list of completed projects.
- **Auth Level:** Public
- **Query Parameters:** `page=1`, `per_page=12`, `service_id=null` (optional filter by service)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "title": "string",
      "slug": "string",
      "description": "string",
      "client": "string",
      "project_date": "date",
      "thumbnail_url": "string",
      "service_id": "integer"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

#### Get Project Detail
- **Method:** `GET`
- **Path:** `/api/public/projects/{slug}`
- **Description:** Retrieve full details of a specific project including gallery, description, and associated service.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "slug": "string",
    "description": "string (HTML)",
    "client": "string",
    "project_date": "date",
    "service": {
      "id": "integer",
      "name": "string",
      "slug": "string"
    },
    "gallery": [
      {
        "id": "integer",
        "image_url": "string",
        "caption": "string"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 500 (Server Error)

### Team

#### List All Team Members
- **Method:** `GET`
- **Path:** `/api/public/team`
- **Description:** Retrieve a paginated list of all team members.
- **Auth Level:** Public
- **Query Parameters:** `page=1`, `per_page=12`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "title": "string",
      "bio": "string",
      "photo_url": "string",
      "email": "string"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

#### Get Team Member Detail
- **Method:** `GET`
- **Path:** `/api/public/team/{id}`
- **Description:** Retrieve detailed profile information for a specific team member.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "title": "string",
    "bio": "string (HTML)",
    "photo_url": "string",
    "email": "string",
    "phone": "string",
    "social_media": {
      "linkedin": "string",
      "twitter": "string"
    }
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 500 (Server Error)

### Blog / News

#### List Blog Posts
- **Method:** `GET`
- **Path:** `/api/public/blog`
- **Description:** Retrieve a reverse-chronologically sorted, paginated list of published blog posts.
- **Auth Level:** Public
- **Query Parameters:** `page=1`, `per_page=10`, `category_id=null` (optional), `tag=null` (optional)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "title": "string",
      "slug": "string",
      "excerpt": "string",
      "author": "string",
      "published_at": "datetime",
      "featured_image_url": "string",
      "category": {
        "id": "integer",
        "name": "string"
      },
      "tags": [
        {
          "id": "integer",
          "name": "string"
        }
      ]
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

#### Get Blog Post Detail
- **Method:** `GET`
- **Path:** `/api/public/blog/{slug}`
- **Description:** Retrieve the full content of a specific published blog post.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "slug": "string",
    "content": "string (HTML)",
    "excerpt": "string",
    "author": "string",
    "published_at": "datetime",
    "featured_image_url": "string",
    "category": {
      "id": "integer",
      "name": "string"
    },
    "tags": [
      {
        "id": "integer",
        "name": "string"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "related_posts": [
      {
        "id": "integer",
        "title": "string",
        "slug": "string",
        "featured_image_url": "string"
      }
    ]
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 500 (Server Error)

#### List Blog Categories
- **Method:** `GET`
- **Path:** `/api/public/blog/categories`
- **Description:** Retrieve all available blog post categories.
- **Auth Level:** Public
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "slug": "string",
      "post_count": "integer"
    }
  ]
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

### Testimonials

#### List Approved Testimonials
- **Method:** `GET`
- **Path:** `/api/public/testimonials`
- **Description:** Retrieve a paginated list of approved testimonials for display on the public site.
- **Auth Level:** Public
- **Query Parameters:** `page=1`, `per_page=10`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "content": "string",
      "client_name": "string",
      "client_company": "string",
      "client_title": "string",
      "image_url": "string",
      "rating": "integer (1-5)"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 500 (Server Error)

### Contact Form

#### Submit Contact Form
- **Method:** `POST`
- **Path:** `/api/public/contact`
- **Description:** Submit a contact form message. Sends an email notification to the admin and stores the submission in the database. Rate-limited to prevent abuse.
- **Auth Level:** Public
- **Rate Limit:** 5 requests per hour per IP
- **Request Body:**
```json
{
  "name": "string (required, max 255)",
  "email": "string (required, valid email)",
  "subject": "string (required, max 255)",
  "message": "string (required, min 10, max 5000)"
}
```
- **Response Body (Success):**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "submission_id": "string (UUID)"
  },
  "message": "Thank you for your message. We will get back to you soon."
}
```
- **Response Body (Error):**
```json
{
  "success": false,
  "error": "validation_error",
  "message": "Validation failed",
  "errors": {
    "email": ["The email field must be a valid email address."],
    "message": ["The message field is required."]
  }
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 429 (Too Many Requests), 500 (Server Error)

---

## Admin API Endpoints

### Authentication

#### Admin Login
- **Method:** `POST`
- **Path:** `/api/admin/login`
- **Description:** Authenticate the administrator and establish a session. Rate-limited to prevent brute-force attacks.
- **Auth Level:** Public
- **Rate Limit:** 5 failed attempts per 15 minutes per IP
- **Request Body:**
```json
{
  "email": "string (required, valid email)",
  "password": "string (required, min 8)"
}
```
- **Response Body (Success):**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "integer",
      "name": "string",
      "email": "string"
    }
  },
  "message": "Login successful"
}
```
- **Response Body (Error):**
```json
{
  "success": false,
  "error": "invalid_credentials",
  "message": "The provided credentials are invalid."
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 429 (Too Many Requests), 500 (Server Error)

#### Admin Logout
- **Method:** `POST`
- **Path:** `/api/admin/logout`
- **Description:** Terminate the admin session.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Current Admin User
- **Method:** `GET`
- **Path:** `/api/admin/user`
- **Description:** Retrieve the currently authenticated admin user information.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "email": "string",
    "created_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

### Admin Dashboard

#### Get Dashboard Overview
- **Method:** `GET`
- **Path:** `/api/admin/dashboard`
- **Description:** Retrieve dashboard overview data including analytics summary, recent contact submissions, and quick stats.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "stats": {
      "total_visitors_this_month": "integer",
      "total_contact_submissions": "integer",
      "published_posts": "integer",
      "total_projects": "integer"
    },
    "recent_submissions": [
      {
        "id": "integer",
        "name": "string",
        "email": "string",
        "subject": "string",
        "submitted_at": "datetime",
        "read": "boolean"
      }
    ],
    "top_pages": [
      {
        "page": "string",
        "views": "integer"
      }
    ]
  }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

### Global Settings Management

#### Get Global Settings
- **Method:** `GET`
- **Path:** `/api/admin/settings`
- **Description:** Retrieve all site-wide settings for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "company_name": "string",
    "logo_url": "string",
    "favicon_url": "string",
    "contact_email": "string",
    "contact_phone": "string",
    "address": "string",
    "social_media": {
      "facebook": "string",
      "twitter": "string",
      "linkedin": "string",
      "instagram": "string"
    },
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Update Global Settings
- **Method:** `PUT`
- **Path:** `/api/admin/settings`
- **Description:** Update site-wide settings.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "company_name": "string (optional)",
  "contact_email": "string (optional, valid email)",
  "contact_phone": "string (optional)",
  "address": "string (optional)",
  "social_media": {
    "facebook": "string (optional, valid URL)",
    "twitter": "string (optional, valid URL)",
    "linkedin": "string (optional, valid URL)",
    "instagram": "string (optional, valid URL)"
  }
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated settings */ },
  "message": "Settings updated successfully"
}
```
- **Status Codes:** 200 (OK), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Upload Logo
- **Method:** `POST`
- **Path:** `/api/admin/settings/logo`
- **Description:** Upload a new company logo to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `logo` file (image, max 5MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "logo_url": "string (S3 URL)"
  },
  "message": "Logo uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Upload Favicon
- **Method:** `POST`
- **Path:** `/api/admin/settings/favicon`
- **Description:** Upload a new favicon to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `favicon` file (image, max 1MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "favicon_url": "string (S3 URL)"
  },
  "message": "Favicon uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

### Page Content Management

#### Get About Us Page
- **Method:** `GET`
- **Path:** `/api/admin/pages/about`
- **Description:** Retrieve the About Us page content for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "content": "string (HTML)",
    "mission": "string",
    "vision": "string",
    "values": [
      {
        "id": "integer",
        "title": "string",
        "description": "string"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Update About Us Page
- **Method:** `PUT`
- **Path:** `/api/admin/pages/about`
- **Description:** Update the About Us page content. Automatically creates a version history entry.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "title": "string (optional)",
  "content": "string (optional, HTML)",
  "mission": "string (optional)",
  "vision": "string (optional)",
  "values": [
    {
      "title": "string",
      "description": "string"
    }
  ],
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated page */ },
  "message": "About Us page updated successfully"
}
```
- **Status Codes:** 200 (OK), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Get Page Version History
- **Method:** `GET`
- **Path:** `/api/admin/pages/{page_type}/versions`
- **Description:** Retrieve all saved versions of a specific page (e.g., `about`, `services`).
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=20`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "version_number": "integer",
      "title": "string",
      "created_at": "datetime",
      "created_by": "string"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Specific Page Version
- **Method:** `GET`
- **Path:** `/api/admin/pages/{page_type}/versions/{version_id}`
- **Description:** Retrieve the content of a specific page version.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "version_number": "integer",
    "title": "string",
    "content": "string (HTML)",
    "created_at": "datetime",
    "created_by": "string"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Rollback Page to Version
- **Method:** `POST`
- **Path:** `/api/admin/pages/{page_type}/versions/{version_id}/rollback`
- **Description:** Restore a page to a previous version. Creates a new version entry for the rollback action.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": { /* restored page */ },
  "message": "Page rolled back to version X successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

### Services Management

#### List Services (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/services`
- **Description:** Retrieve a paginated list of all services for admin management.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=15`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "slug": "string",
      "description": "string",
      "icon_url": "string",
      "image_url": "string",
      "created_at": "datetime",
      "updated_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Service Detail (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/services/{id}`
- **Description:** Retrieve full details of a specific service for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "slug": "string",
    "description": "string (HTML)",
    "icon_url": "string",
    "image_url": "string",
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Create Service
- **Method:** `POST`
- **Path:** `/api/admin/services`
- **Description:** Create a new service.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (required, max 255)",
  "description": "string (required, HTML)",
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* created service */ },
  "message": "Service created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Update Service
- **Method:** `PUT`
- **Path:** `/api/admin/services/{id}`
- **Description:** Update an existing service.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (optional)",
  "description": "string (optional, HTML)",
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated service */ },
  "message": "Service updated successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Service
- **Method:** `DELETE`
- **Path:** `/api/admin/services/{id}`
- **Description:** Delete a service. Associated projects are not deleted but lose the service reference.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Service deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Upload Service Icon
- **Method:** `POST`
- **Path:** `/api/admin/services/{id}/icon`
- **Description:** Upload an icon image for a service to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `icon` file (image, max 2MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "icon_url": "string (S3 URL)"
  },
  "message": "Icon uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Upload Service Image
- **Method:** `POST`
- **Path:** `/api/admin/services/{id}/image`
- **Description:** Upload a featured image for a service to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `image` file (image, max 5MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "image_url": "string (S3 URL)"
  },
  "message": "Image uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

### Portfolio / Projects Management

#### List Projects (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/projects`
- **Description:** Retrieve a paginated list of all projects for admin management.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=15`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "title": "string",
      "slug": "string",
      "client": "string",
      "project_date": "date",
      "service_id": "integer",
      "thumbnail_url": "string",
      "created_at": "datetime",
      "updated_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Project Detail (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/projects/{id}`
- **Description:** Retrieve full details of a specific project for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "slug": "string",
    "description": "string (HTML)",
    "client": "string",
    "project_date": "date",
    "service_id": "integer",
    "thumbnail_url": "string",
    "gallery": [
      {
        "id": "integer",
        "image_url": "string",
        "caption": "string",
        "order": "integer"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Create Project
- **Method:** `POST`
- **Path:** `/api/admin/projects`
- **Description:** Create a new project.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "title": "string (required, max 255)",
  "description": "string (required, HTML)",
  "client": "string (required, max 255)",
  "project_date": "date (required)",
  "service_id": "integer (optional)",
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* created project */ },
  "message": "Project created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Update Project
- **Method:** `PUT`
- **Path:** `/api/admin/projects/{id}`
- **Description:** Update an existing project.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "title": "string (optional)",
  "description": "string (optional, HTML)",
  "client": "string (optional)",
  "project_date": "date (optional)",
  "service_id": "integer (optional)",
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated project */ },
  "message": "Project updated successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Project
- **Method:** `DELETE`
- **Path:** `/api/admin/projects/{id}`
- **Description:** Delete a project and all associated gallery images.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Project deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Upload Project Thumbnail
- **Method:** `POST`
- **Path:** `/api/admin/projects/{id}/thumbnail`
- **Description:** Upload a thumbnail image for a project to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `thumbnail` file (image, max 5MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "thumbnail_url": "string (S3 URL)"
  },
  "message": "Thumbnail uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Add Gallery Image to Project
- **Method:** `POST`
- **Path:** `/api/admin/projects/{id}/gallery`
- **Description:** Add an image to a project's gallery on AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `image` file (image, max 5MB) and optional `caption` (string)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "image_url": "string (S3 URL)",
    "caption": "string"
  },
  "message": "Gallery image added successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Gallery Image
- **Method:** `DELETE`
- **Path:** `/api/admin/projects/{project_id}/gallery/{gallery_id}`
- **Description:** Remove an image from a project's gallery and delete it from AWS S3.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Gallery image deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Reorder Gallery Images
- **Method:** `POST`
- **Path:** `/api/admin/projects/{id}/gallery/reorder`
- **Description:** Update the display order of gallery images.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "gallery_order": [
    {
      "id": "integer",
      "order": "integer"
    }
  ]
}
```
- **Response Body:**
```json
{
  "success": true,
  "message": "Gallery order updated successfully"
}
```
- **Status Codes:** 200 (OK), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

### Team Management

#### List Team Members (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/team`
- **Description:** Retrieve a paginated list of all team members for admin management.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=15`
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "title": "string",
      "email": "string",
      "photo_url": "string",
      "created_at": "datetime",
      "updated_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Team Member Detail (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/team/{id}`
- **Description:** Retrieve full details of a specific team member for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "title": "string",
    "bio": "string (HTML)",
    "email": "string",
    "phone": "string",
    "photo_url": "string",
    "social_media": {
      "linkedin": "string",
      "twitter": "string"
    },
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Create Team Member
- **Method:** `POST`
- **Path:** `/api/admin/team`
- **Description:** Create a new team member profile.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (required, max 255)",
  "title": "string (required, max 255)",
  "bio": "string (optional, HTML)",
  "email": "string (required, valid email)",
  "phone": "string (optional)",
  "social_media": {
    "linkedin": "string (optional, valid URL)",
    "twitter": "string (optional, valid URL)"
  }
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* created team member */ },
  "message": "Team member created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Update Team Member
- **Method:** `PUT`
- **Path:** `/api/admin/team/{id}`
- **Description:** Update an existing team member profile.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (optional)",
  "title": "string (optional)",
  "bio": "string (optional, HTML)",
  "email": "string (optional, valid email)",
  "phone": "string (optional)",
  "social_media": {
    "linkedin": "string (optional, valid URL)",
    "twitter": "string (optional, valid URL)"
  }
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated team member */ },
  "message": "Team member updated successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Team Member
- **Method:** `DELETE`
- **Path:** `/api/admin/team/{id}`
- **Description:** Delete a team member profile.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Team member deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Upload Team Member Photo
- **Method:** `POST`
- **Path:** `/api/admin/team/{id}/photo`
- **Description:** Upload a profile photo for a team member to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `photo` file (image, max 5MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "photo_url": "string (S3 URL)"
  },
  "message": "Photo uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

### Blog Management

#### List Blog Posts (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/blog`
- **Description:** Retrieve a paginated list of all blog posts (including drafts) for admin management.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=15`, `status=null` (optional: `draft`, `published`, `archived`)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "title": "string",
      "slug": "string",
      "excerpt": "string",
      "author": "string",
      "status": "string (draft|published|archived)",
      "published_at": "datetime|null",
      "featured_image_url": "string",
      "created_at": "datetime",
      "updated_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Blog Post Detail (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/blog/{id}`
- **Description:** Retrieve full details of a specific blog post for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "title": "string",
    "slug": "string",
    "content": "string (HTML)",
    "excerpt": "string",
    "author": "string",
    "status": "string (draft|published|archived)",
    "published_at": "datetime|null",
    "featured_image_url": "string",
    "category_id": "integer|null",
    "tags": [
      {
        "id": "integer",
        "name": "string"
      }
    ],
    "meta_title": "string",
    "meta_description": "string",
    "og_image_url": "string",
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Create Blog Post
- **Method:** `POST`
- **Path:** `/api/admin/blog`
- **Description:** Create a new blog post.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "title": "string (required, max 255)",
  "content": "string (required, HTML)",
  "excerpt": "string (required, max 500)",
  "author": "string (required, max 255)",
  "status": "string (required: draft|published)",
  "category_id": "integer (optional)",
  "tags": [
    {
      "id": "integer (optional, for existing tags)",
      "name": "string (optional, for new tags)"
    }
  ],
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* created blog post */ },
  "message": "Blog post created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Update Blog Post
- **Method:** `PUT`
- **Path:** `/api/admin/blog/{id}`
- **Description:** Update an existing blog post.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "title": "string (optional)",
  "content": "string (optional, HTML)",
  "excerpt": "string (optional, max 500)",
  "author": "string (optional)",
  "status": "string (optional: draft|published|archived)",
  "category_id": "integer (optional)",
  "tags": [
    {
      "id": "integer (optional)",
      "name": "string (optional)"
    }
  ],
  "meta_title": "string (optional)",
  "meta_description": "string (optional, max 160)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated blog post */ },
  "message": "Blog post updated successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Blog Post
- **Method:** `DELETE`
- **Path:** `/api/admin/blog/{id}`
- **Description:** Delete a blog post permanently.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Blog post deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Upload Blog Featured Image
- **Method:** `POST`
- **Path:** `/api/admin/blog/{id}/featured-image`
- **Description:** Upload a featured image for a blog post to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `image` file (image, max 5MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "featured_image_url": "string (S3 URL)"
  },
  "message": "Featured image uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### List Blog Categories
- **Method:** `GET`
- **Path:** `/api/admin/blog/categories`
- **Description:** Retrieve all available blog post categories.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "slug": "string",
      "post_count": "integer"
    }
  ]
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Create Blog Category
- **Method:** `POST`
- **Path:** `/api/admin/blog/categories`
- **Description:** Create a new blog post category.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (required, max 255)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "slug": "string"
  },
  "message": "Category created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Blog Category
- **Method:** `DELETE`
- **Path:** `/api/admin/blog/categories/{id}`
- **Description:** Delete a blog post category. Posts in this category lose the category reference.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Category deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### List Blog Tags
- **Method:** `GET`
- **Path:** `/api/admin/blog/tags`
- **Description:** Retrieve all available blog post tags.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "slug": "string",
      "post_count": "integer"
    }
  ]
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Create Blog Tag
- **Method:** `POST`
- **Path:** `/api/admin/blog/tags`
- **Description:** Create a new blog post tag.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "name": "string (required, max 255)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "slug": "string"
  },
  "message": "Tag created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Blog Tag
- **Method:** `DELETE`
- **Path:** `/api/admin/blog/tags/{id}`
- **Description:** Delete a blog post tag. Posts with this tag lose the tag reference.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Tag deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

### Testimonials Management

#### List Testimonials (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/testimonials`
- **Description:** Retrieve a paginated list of all testimonials (including unapproved) for admin management.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=15`, `status=null` (optional: `approved`, `pending`)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "content": "string",
      "client_name": "string",
      "client_company": "string",
      "rating": "integer (1-5)",
      "status": "string (approved|pending)",
      "created_at": "datetime",
      "updated_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Testimonial Detail (Admin)
- **Method:** `GET`
- **Path:** `/api/admin/testimonials/{id}`
- **Description:** Retrieve full details of a specific testimonial for editing.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "content": "string",
    "client_name": "string",
    "client_company": "string",
    "client_title": "string",
    "rating": "integer (1-5)",
    "image_url": "string",
    "status": "string (approved|pending)",
    "created_at": "datetime",
    "updated_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Create Testimonial
- **Method:** `POST`
- **Path:** `/api/admin/testimonials`
- **Description:** Create a new testimonial.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "content": "string (required, max 1000)",
  "client_name": "string (required, max 255)",
  "client_company": "string (required, max 255)",
  "client_title": "string (optional, max 255)",
  "rating": "integer (required, 1-5)",
  "status": "string (required: approved|pending)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* created testimonial */ },
  "message": "Testimonial created successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Update Testimonial
- **Method:** `PUT`
- **Path:** `/api/admin/testimonials/{id}`
- **Description:** Update an existing testimonial.
- **Auth Level:** Admin
- **Request Body:**
```json
{
  "content": "string (optional)",
  "client_name": "string (optional)",
  "client_company": "string (optional)",
  "client_title": "string (optional)",
  "rating": "integer (optional, 1-5)",
  "status": "string (optional: approved|pending)"
}
```
- **Response Body:**
```json
{
  "success": true,
  "data": { /* updated testimonial */ },
  "message": "Testimonial updated successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Testimonial
- **Method:** `DELETE`
- **Path:** `/api/admin/testimonials/{id}`
- **Description:** Delete a testimonial permanently.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Testimonial deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Upload Testimonial Image
- **Method:** `POST`
- **Path:** `/api/admin/testimonials/{id}/image`
- **Description:** Upload a client image for a testimonial to AWS S3.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `image` file (image, max 3MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "image_url": "string (S3 URL)"
  },
  "message": "Image uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

### Contact Submissions Management

#### List Contact Submissions
- **Method:** `GET`
- **Path:** `/api/admin/contact-submissions`
- **Description:** Retrieve a paginated list of all contact form submissions.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=20`, `status=null` (optional: `unread`, `read`, `archived`)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "name": "string",
      "email": "string",
      "subject": "string",
      "message": "string",
      "status": "string (unread|read|archived)",
      "submitted_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Get Contact Submission Detail
- **Method:** `GET`
- **Path:** `/api/admin/contact-submissions/{id}`
- **Description:** Retrieve full details of a specific contact submission. Automatically marks as read.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "email": "string",
    "subject": "string",
    "message": "string",
    "status": "string (unread|read|archived)",
    "submitted_at": "datetime"
  }
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Archive Contact Submission
- **Method:** `PUT`
- **Path:** `/api/admin/contact-submissions/{id}/archive`
- **Description:** Mark a contact submission as archived.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Submission archived successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

#### Delete Contact Submission
- **Method:** `DELETE`
- **Path:** `/api/admin/contact-submissions/{id}`
- **Description:** Delete a contact submission permanently.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "Submission deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

### Media Library Management

#### List Media Files
- **Method:** `GET`
- **Path:** `/api/admin/media`
- **Description:** Retrieve a paginated list of all uploaded media files from AWS S3.
- **Auth Level:** Admin
- **Query Parameters:** `page=1`, `per_page=20`, `type=null` (optional: `image`, `video`)
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "data": [
    {
      "id": "integer",
      "filename": "string",
      "url": "string (S3 URL)",
      "type": "string (image|video)",
      "size": "integer (bytes)",
      "uploaded_at": "datetime"
    }
  ],
  "pagination": { /* standard pagination */ }
}
```
- **Status Codes:** 200 (OK), 401 (Unauthorized), 500 (Server Error)

#### Upload Media File
- **Method:** `POST`
- **Path:** `/api/admin/media/upload`
- **Description:** Upload a media file to AWS S3. Supports images and videos.
- **Auth Level:** Admin
- **Request Body:** `multipart/form-data` with `file` (image or video, max 50MB)
- **Response Body:**
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "filename": "string",
    "url": "string (S3 URL)",
    "type": "string (image|video)",
    "size": "integer (bytes)"
  },
  "message": "File uploaded successfully"
}
```
- **Status Codes:** 201 (Created), 422 (Unprocessable Entity), 401 (Unauthorized), 500 (Server Error)

#### Delete Media File
- **Method:** `DELETE`
- **Path:** `/api/admin/media/{id}`
- **Description:** Delete a media file from AWS S3 and the database.
- **Auth Level:** Admin
- **Request Body:** None
- **Response Body:**
```json
{
  "success": true,
  "message": "File deleted successfully"
}
```
- **Status Codes:** 200 (OK), 404 (Not Found), 401 (Unauthorized), 500 (Server Error)

---

## Error Handling & Status Codes

All endpoints follow standard HTTP status codes:

| Code | Meaning | Example Scenario |
|:---|:---|:---|
| **200** | OK | Successful GET, PUT, or DELETE request. |
| **201** | Created | Successful POST request creating a new resource. |
| **400** | Bad Request | Malformed request syntax or invalid parameters. |
| **401** | Unauthorized | Missing or invalid authentication credentials. |
| **403** | Forbidden | Authenticated but lacks permission (not applicable for single-admin system). |
| **404** | Not Found | Requested resource does not exist. |
| **422** | Unprocessable Entity | Request validation failed (e.g., invalid email format). |
| **429** | Too Many Requests | Rate limit exceeded (login, contact form). |
| **500** | Server Error | Internal server error or unexpected exception. |

---

## Rate Limiting

The following endpoints are rate-limited to prevent abuse:

| Endpoint | Limit | Window |
|:---|:---|:---|
| `POST /api/admin/login` | 5 failed attempts | 15 minutes per IP |
| `POST /api/public/contact` | 5 requests | 1 hour per IP |

Rate limit information is included in response headers:
```
X-RateLimit-Limit: 5
X-RateLimit-Remaining: 3
X-RateLimit-Reset: 1234567890
```

---

## CORS & Security Headers

All API responses include the following security headers:

```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'
```

CORS is configured to allow requests only from the same origin (same-site requests only).

---

## File Upload Specifications

All file uploads are stored on AWS S3 with the following specifications:

| File Type | Max Size | Allowed Formats | S3 Path |
|:---|:---|:---|:---|
| **Images** | 5 MB | JPG, PNG, WebP | `/uploads/images/{year}/{month}/{filename}` |
| **Videos** | 50 MB | MP4, WebM | `/uploads/videos/{year}/{month}/{filename}` |
| **Logo/Favicon** | 1-5 MB | PNG, ICO, SVG | `/uploads/branding/{filename}` |

All files are served via CloudFront CDN for optimal performance. File names are sanitized and renamed with a UUID prefix to prevent conflicts.