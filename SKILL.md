---
name: generate-prd
description: Menghasilkan 9 dokumen PRD lengkap dan konsisten (PRD, REQUIREMENTS, API, ARCHITECTURE, DATABASE, DEPLOYMENT, DESIGN_SYSTEM, ROADMAP, USER_FLOW) dengan kedalaman profesional setara dokumen ProfilKorp. Gunakan saat user minta "buat PRD", "generate dokumentasi proyek", "buat 9 file prd", "buat requirement spec lengkap", atau ingin membuat dokumentasi teknis menyeluruh untuk proyek apa pun.
---

# Generate PRD

Skill ini menghasilkan 9 dokumen markdown yang utuh, detail, dan saling konsisten untuk satu proyek, ditulis dalam Bahasa Indonesia (istilah teknis tetap English) — meniru struktur dan kedalaman file PRD ProfilKorp yang tersimpan di `templates/`. Setiap dokumen adaptasi proyek, bukan salinan ProfilKorp.

## Output 9 File

| # | File | Isi Inti |
|:--|:-----|:---------|
| 1 | `PRD.md` | Vision, scope, roles, Functional Requirements (FR), NFR, tech stack, KPI, risk, constraints, out of scope |
| 2 | `REQUIREMENTS.md` | FR & NFR dalam format "the system SHALL/MUST/SHOULD", constraints, assumptions |
| 3 | `API.md` | Auth, response format, seluruh endpoint publik & admin (method, path, request, response JSON, status code) |
| 4 | `ARCHITECTURE.md` | System overview, diagram mermaid, component breakdown, sequence diagram, deployment, security, perf, scaling |
| 5 | `DATABASE.md` | ERD mermaid, definisi tiap tabel, schema mengikuti stack (Prisma/Eloquent/DDL), indexing, integrity, migration, perf, security |
| 6 | `DEPLOYMENT.md` | Environment tiers, VPS setup, Nginx, SSL, CI/CD, monitoring, backup, rollback, security hardening, runbook |
| 7 | `DESIGN_SYSTEM.md` | Brand & visual identity, UX goals, color palette, typography, spacing, responsive breakpoints, motion, accessibility |
| 8 | `ROADMAP.md` | Phased plan, MVP (P0/P1/P2), milestones, dependencies, risks, resource allocation, success criteria |
| 9 | `USER_FLOW.md` | 10-12 flow utama total, dikelompokkan per actor, tiap flow: Trigger, Pre-conditions, Post-conditions, Flow Table |

## Workflow

1. **Intake proyek.** Tanyakan hal penting satu per satu hingga lengkap. Jangan pernah menebak field penting — tanya jika belum jelas. Field penting yang WAJIB dikonfirmasi:
   - Nama proyek (untuk folder output `prd-<nama>` dan judul dokumen).
   - Jenis produk (corporate website, aplikasi internal, API service, dsb).
   - Tech stack (backend, frontend, database, storage, hosting, auth).
   - Scope & fitur utama (publik + admin/backend).
   - Target user & peran/roles.
   - Constraint khusus (single tenant, single admin, dsb).
   - Tanyakan kembali bila field penting (nama, jenis, stack, scope, roles, lokasi output) belum jelas. Jika prompt awal belum lengkap, wajib bertanya balik sebelum generate — jangan langsung menulis file.

2. **Rangkum & konfirmasi balik (WAJIB).** Sebelum menulis file apa pun, tampilkan ringkasan tabel proyek (nama, jenis produk, tech stack, scope/fitur, roles, constraint, lokasi output `prd-<nama>/`). Lalu tanyakan eksplisit ke user: *"Konfirmasi? Ada yang mau diubah?"*. **WAJIB tunggu jawaban user.** Mulai generate hanya setelah user konfirmasi; jika ada koreksi, perbaiki ringkasan dan konfirmasi ulang.

3. **Generate, satu file per template, dalam urutan yang benar.** Urutan wajib: PRD → REQUIREMENTS → DATABASE → API → ARCHITECTURE → DEPLOYMENT → DESIGN_SYSTEM → ROADMAP → USER_FLOW. Dokumen awal menjadi sumber kebenaran (nama entitas, FR ID, field) untuk dokumen berikutnya. Untuk tiap file:
   - Baca template terkait di `templates/` (mis. `templates/PRD.md`) sebagai referensi struktur, section, tabel, serta kedalaman contoh JSON/mermaid.
   - Tulis file output adaptasi proyek ke folder `prd-<nama>/<FILE>.md`.
   - Ikuti kontrak konsistensi di [REFERENCE.md](REFERENCE.md) — wajib dibaca sebelum mulai.

4. **Validasi akhir.** Periksa: semua FR di PRD muncul di REQUIREMENTS & API; nama entitas DATABASE konsisten dengan API/ARCHITECTURE; tidak ada sisa template (`ProfilKorp`, `Laravel`, `MySQL`, `AWS S3`, `Sanctum`, `Inertia`, `React`, `Tailwind`, `Inter`) yang tertinggal pada proyek dengan stack berbeda.

## Aturan Kedalaman & Konsistensi

- Judul tiap file: `# NAMA.md: <Proyek>` (khusus PRD: `# PRD: <Proyek>`).
- Tulis dalam Bahasa Indonesia, tone profesional-teknis. Istilah teknis tetap English (FR ID, nama entitas/tabel/kolom, endpoint/path, field JSON, status code, nama teknologi).
- Gunakan tabel markdown untuk data terstruktur, mermaid untuk `graph TD` (ARCHITECTURE) dan `erDiagram` (DATABASE), JSON snippet untuk contoh request/response.
- Penomoran FR-01..FR-n harus terurut dan identik antar dokumen.
- Level detail harus menyamai template-nya: tiap endpoint punya JSON response, tiap tabel punya kolom + tipe + constraint, tiap flow punya Flow Table.
- Kontrak detail antar dokumen: lihat [REFERENCE.md](REFERENCE.md).

## Contoh Prompt User

- **Minimal (skill akan tanya balik detail yang kurang):** "Buat PRD lengkap untuk aplikasi booking klinik."
- **Sedang:** "Generate 9 file prd untuk startup marketplace, stack Laravel + Vue + PostgreSQL, fitur utama: catalogue, cart, payment."
- **Detail (langsung konfirmasi):** "Buat PRD aplikasi internal logistik bernama ShipTrack, backend Laravel, frontend React, DB MySQL, single admin, perlu fitur tracking pesanan + laporan."

Saat prompt kurang lengkap, skill akan menampilkan ringkasan proyek dan bertanya balik "Konfirmasi? Ada yang mau diubah?" sebelum membuat file.