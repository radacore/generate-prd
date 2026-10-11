# generate-prd

Skill [OpenCode](https://opencode.ai) untuk menghasilkan **9 dokumen PRD lengkap dan saling konsisten** dalam satu kali jalan — dari vision produk sampai user flow, siap dipakai tim engineering.

Output: folder `prd-<nama-proyek>/` berisi 9 file markdown.

| # | File | Isi Inti |
|:--|:-----|:---------|
| 1 | `PRD.md` | Vision, scope, roles, Functional Requirements (FR), NFR, tech stack, KPI, risk, constraints |
| 2 | `REQUIREMENTS.md` | FR & NFR format "the system SHALL/MUST/SHOULD", constraints, assumptions |
| 3 | `API.md` | Auth, response format, seluruh endpoint publik & admin (request/response JSON, status code) |
| 4 | `ARCHITECTURE.md` | System overview, diagram mermaid, component breakdown, sequence diagram, security, scaling |
| 5 | `DATABASE.md` | ERD mermaid, definisi tabel, schema mengikuti stack, indexing, integrity, migration |
| 6 | `DEPLOYMENT.md` | Environment tiers, VPS setup, Nginx, SSL, CI/CD, monitoring, backup, runbook |
| 7 | `DESIGN_SYSTEM.md` | Brand identity, UX goals, color palette, typography, spacing, breakpoints, accessibility |
| 8 | `ROADMAP.md` | Phased plan, MVP (P0/P1/P2), milestones, dependencies, risks, success criteria |
| 9 | `USER_FLOW.md` | 10-12 flow utama per actor, tiap flow lengkap dengan Flow Table |

## Instalasi

Clone langsung ke folder skills OpenCode:

```bash
git clone https://github.com/radacore/generate-prd.git \
  ~/.config/opencode/skills/generate-prd
```

Lokasi folder skills OpenCode:

| Scope | Path |
|:------|:-----|
| Global (semua proyek) | `~/.config/opencode/skills/generate-prd/` |
| Per-proyek | `<project>/.opencode/skills/generate-prd/` |

> Nama folder **harus** `generate-prd` — sama dengan field `name:` di frontmatter `SKILL.md`.

Setelah clone, **restart OpenCode** agar skill terdeteksi.

### Manual (tanpa git)

1. Download repo sebagai ZIP, ekstrak.
2. Pindahkan folder `generate-prd/` ke `~/.config/opencode/skills/`.
3. Pastikan `SKILL.md`, `REFERENCE.md`, dan folder `templates/` ikut terbawa.
4. Restart OpenCode.

## Cara Pakai

Panggil skill lewat prompt biasa. Trigger yang dikenali: *"buat PRD"*, *"generate dokumentasi proyek"*, *"buat 9 file prd"*, *"buat requirement spec lengkap"*.

**Minimal** — skill akan bertanya balik detail yang kurang:

```
Buat PRD lengkap untuk aplikasi booking klinik.
```

**Sedang** — sebagian detail sudah disebut:

```
Generate 9 file prd untuk startup marketplace, stack Laravel + Vue + PostgreSQL,
fitur utama: catalogue, cart, payment.
```

**Detail** — langsung masuk konfirmasi:

```
Buat PRD aplikasi internal logistik bernama ShipTrack, backend Laravel,
frontend React, DB MySQL, single admin, perlu fitur tracking pesanan + laporan.
```

## Alur Kerja Skill

1. **Intake** — skill menanyakan field penting satu per satu: nama proyek, jenis produk, tech stack, scope & fitur, target user & roles, constraint, lokasi output. Tidak menebak field penting.
2. **Konfirmasi** — menampilkan ringkasan tabel proyek dan menunggu jawaban eksplisit sebelum menulis file.
3. **Generate** — menulis 9 file berurutan (PRD → REQUIREMENTS → DATABASE → API → ARCHITECTURE → DEPLOYMENT → DESIGN_SYSTEM → ROADMAP → USER_FLOW). Dokumen awal jadi sumber kebenaran untuk dokumen berikutnya.
4. **Validasi** — cek konsistensi FR ID, nama entitas, dan tidak ada sisa template dari stack lain.

## Struktur Repo

```
generate-prd/
├── SKILL.md          # Definisi skill + workflow (dibaca OpenCode)
├── REFERENCE.md      # Kontrak konsistensi antar 9 dokumen
├── README.md         # Dokumen ini
└── templates/        # Acuan struktur & kedalaman tiap dokumen
    ├── PRD.md
    ├── REQUIREMENTS.md
    ├── API.md
    ├── ARCHITECTURE.md
    ├── DATABASE.md
    ├── DEPLOYMENT.md
    ├── DESIGN_SYSTEM.md
    ├── ROADMAP.md
    └── USER_FLOW.md
```

`templates/` wajib ada — tanpa itu skill tetap jalan tapi kedalaman output menurun karena kehilangan acuan.

## Update

```bash
cd ~/.config/opencode/skills/generate-prd
git pull
```

## Bahasa Output

Dokumen ditulis dalam **Bahasa Indonesia** dengan tone profesional-teknis. Istilah teknis tetap English (FR ID, nama tabel/kolom, endpoint, field JSON, status code, nama teknologi).
