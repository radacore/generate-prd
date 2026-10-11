# Referensi: Kontrak Konsistensi & Struktur Per Dokumen

Dokumen ini mendefinisikan bagaimana 9 file saling terkait agar output adaptasi proyek konsisten. Selalu baca sebelum mulai generate.

## Kebenaran Sumber (Source of Truth)

| File | Menyediakan kanon untuk | Dikonsumsi di |
|:-----|:------------------------|:--------------|
| PRD.md | FR ID (FR-01..FR-n), roles, scope, tech stack, fitur | Semua file lain |
| REQUIREMENTS.md | Kata kerja kewajiban tiap FR (SHALL/MUST/SHOULD) | API (endpoint), USER_FLOW (flow) |
| API.md | Nama endpoint, parameter, field response | USER_FLOW (aksi), ARCHITECTURE (endpoint) |
| DATABASE.md | Nama entitas/tabel, kolom, tipe field | API (field = kolom), ARCHITECTURE (ERD) |

**Urutan generate wajib** PRD → REQUIREMENTS → DATABASE → API → ARCHITECTURE → … (sesuai SKILL.md) memastikan kebenaran mengalir satu arah: entitas/kolom DATABASE ditetapkan dulu, baru endpoint API memakai field yang match. Jangan generate dokumen turunan sebelum sumbernya selesai.

## Kontrak Konsistensi Wajib

1. **FR ID seragam.** Setiap FR muncul di PRD, REQUIREMENTS, API, dan USER_FLOW dengan ID sama (FR-01..FR-n, terurut). Tidak boleh ada FR hanya di satu file.
2. **Nama entitas tunggal.** Nama entitas (mis. `BLOG_POST`, `PORTFOLIO_PROJECT`) di DATABASE harus identik di ARCHITECTURE (ERD/component) dan API (section, resource). Gunakan SNAKE_CASE untuk entitas, snake_case untuk kolom DB dan field JSON agar match 1:1.
3. **Field JSON = kolom tabel.** Contoh response JSON di API harus memakai nama field yang match kolom DATABASE (mis. kolom `published_at` → field `published_at`).
4. **Roles konsisten.** Role/daftar permission di PRD dan REQUIREMENTS harus sama persis.
5. **Stack konsisten.** Tech stack PRD → ARCHITECTURE (component), DEPLOYMENT (tooling), DATABASE (Prisma/provider), API (auth mechanism).
6. **Tanpa sisa template.** Jangan ada tersisa nama "ProfilKorp", Laravel, MySQL, AWS S3, dsb di output kecuali memang stack proyek tersebut.

## Judul & Style Per File

| File | Judul baris 1 | Front matter | Sumber style |
|:-----|:--------------|:-------------|:-------------|
| PRD.md | `# PRD: <Nama>` | tidak ada | `templates/PRD.md` |
| REQUIREMENTS.md | `# REQUIREMENTS.md: <Nama>` | tidak ada | `templates/REQUIREMENTS.md` |
| API.md | `# API.md: <Nama>` | tidak ada | `templates/API.md` |
| ARCHITECTURE.md | `# ARCHITECTURE.md: <Nama>` | tidak ada | `templates/ARCHITECTURE.md` |
| DATABASE.md | `# DATABASE.md: <Nama>` | tidak ada | `templates/DATABASE.md` |
| DEPLOYMENT.md | `# DEPLOYMENT.md: <Nama>` | tidak ada | `templates/DEPLOYMENT.md` |
| DESIGN_SYSTEM.md | `# DESIGN_SYSTEM.md: <Nama>` | tidak ada | `templates/DESIGN_SYSTEM.md` |
| ROADMAP.md | `# ROADMAP.md: <Nama>` | tidak ada | `templates/ROADMAP.md` |
| USER_FLOW.md | `# USER_FLOW.md: <Nama>` | tidak ada | `templates/USER_FLOW.md` |

Bahasa konten: Bahasa Indonesia. Tone: profesional-teknis. Istilah teknis tetap English (FR ID, entitas/tabel/kolom, endpoint, field JSON, status code, nama stack).

## Persyaratan Kedalaman Minimum Per Dokumen

### PRD.md
- Executive Summary & Product Vision; Problem Statement & Target Users (kategori user).
- System Scope & User Roles (tabel) → bisa single-role jika hanya 1 pengguna (mis. admin).
- Functional Requirements: split "Public-Facing" dan "Admin Dashboard/Backend", tiap FR 1 bullet dengan kriteria detail.
- Non-Functional Requirements (tabel Kategori/Requirement/Metric‑Target), memorable target (mis. LCP < 2.5s).
- Technology Stack & Rationale (tabel Komponen/Teknologi/Alasan).
- Success Metrics & KPIs; Risk Analysis & Mitigation; Constraints & Assumptions; Out of Scope.

### REQUIREMENTS.md
- Pindahkan tiap FR PRD ke format: `**FR-0X: <Judul>**` + 1 kalimat `SHALL`, lalu bullet `MUST`/`SHOULD` bersyarat.
- Bagi `## 1. Functional Requirements` / `### 1.1.` & `### 1.2.` (disesuaikan bagian), `## 2. Non-Functional`, `## 3. Technical Constraints`, `## 4. Assumptions`.

### API.md
- Kesetaraan FR → endpoint. Tiap resource publik & admin punya section `####`.
- Format endpoint (meniru `templates/API.md`):
  ```
  #### <Aksi> <Resource>
  - **Method:** `GET`/`POST`/`PUT`/`PATCH`/`DELETE`
  - **Path:** `/api/public/...` atau `/api/admin/...`
  - **Description:** ...
  - **Auth Level:** Public/Admin
  - **Request Body:** None / <daftar field>
  - **Response Body:** ```json {...}```
  - **Status Codes:** 200 (OK), 4xx, 500 ...
  ```
- Sertakan: Authentication & Authorization; Standard Response & Pagination Formats; Error Handling & Status Codes; Rate Limiting; CORS & Security Headers; File Upload Specifications.

### ARCHITECTURE.md
- Diagram `graph TD` mermaid penuh (visitor/admin → app server → DB/S3/cache).
- Component Breakdown per teknologi; Critical Flow Sequence Diagrams (`sequenceDiagram`); Deployment Strategy; Data Flow; Security Architecture; Performance; Scalability; Dev & Build Pipeline.

### DATABASE.md
- `erDiagram` mermaid berisi relasi antar entitas; Table Definitions tiap tabel: nama, kolom, tipe, constraint, index, relasi.
- Schema penuh mengikuti stack (Prisma/Eloquent/SQL DDL — model per tabel, tipe sesuai DB).
- Indexing Strategy, Data Integrity & Constraints, Migration & Seeding, Performance, Security & Compliance.

### DEPLOYMENT.md
- Environment Strategy (tier Development/Staging/Production + tabel).
- Infrastruktur (VPS config, Nginx, SSL), deployment steps (bash code block), AWS S3/objek storage config, CI/CD (GitHub Actions), Monitoring (Sentry/log), Caching (Redis), Backup, Rollback, Security Hardening, Disaster Recovery (RTO/RPO), Runbook.
- Sesuaikan platform hosting/CI dengan stack proyek, bukan selalu VPS/GitHub.

### DESIGN_SYSTEM.md
- Brand & Visual Identity; User Experience Goals (dengan angka terukur).
- Color Palette (tabel warna + CSS variable), Typography (scale/berat), UI Components & Spacing, Responsive Breakpoints, Screen Priorities, Interaction & Motion (CSS snippet), Accessibility (contrast, keyboard, checklist).

### ROADMAP.md
- Phased Delivery Plan (tabel fase/durasi/goals/deliverables) + disclaimer asumsi tim.
- MVP Feature List (P0/P1/P2), Milestones, Dependencies (eksternal/internal), Risks & Mitigation, Technical Milestones per Phase, Success Criteria & Go-Live Checklist, Post-Launch Roadmap, Resource Allocation, Communication & Governance.
- Jumlah fase disesuaikan besaran scope proyek (bukan selalu 5+2).

### USER_FLOW.md
- Mulai `## Overview`.
- 10-12 flow utama total, dikelompokkan per actor (Public User, Administrator/internal).
- Tiap flow: `## Flow N: <Pesan>`, lalu `### Trigger`, `### Pre-conditions`, `### Post-conditions`, `### Flow Table` (tabel Step/Actor/Action).
- Akhiri `## Summary of Key User Flows`.
- Flow harus membahas aksi yang memang ada di PRD (mapping dari FR).

## Cara Menggunakan Template

Template di `templates/` adalah **acuan kedalaman & format**, bukan konten. Strategi:

1. Baca satu template lengkap sebelum menulis dokumen padanannya.
2. Tahan section order, kalimat pembuka, jenis tabel/JSON/mermaid yang dipakai.
3. Ganti seluruh konten spesifik ProfilKorp dengan adaptasi proyek (entitas, stack, fitur, flow).
4. Jika proyek lebih kecil, kurangi jumlah entitas/flow/endpoint secara wajar, tapi pertahankan kedalaman yang sama per bagian yang ada.