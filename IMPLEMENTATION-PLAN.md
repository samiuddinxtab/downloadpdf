# Strategic Multi-Task Implementation Plan
## Digital Islamic PDF Library

**Version:** 1.0  
**Based on:** PRD Version 5.0 — Final Repository Specification  
**Goal:** Comprehensive, dependency-ordered task breakdown for building a mobile-first, OAIS-aligned Digital Islamic PDF Library with Urdu RTL interface, SHA-256 preservation, controlled vocabulary tags, authority records, and static architecture.

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Phases Overview](#phases-overview)
3. [Detailed Task Breakdown](#detailed-task-breakdown)
4. [Dependency Graph](#dependency-graph)
5. [Files to Create/Modify](#files-to-createmodify)
6. [Risk Assessment](#risk-assessment)
7. [Success Metrics](#success-metrics)

---

## Executive Summary

This implementation plan breaks down the Digital Islamic PDF Library build into **6 sequential phases** with **47 discrete tasks**. Each task includes:

- **Entrance Criteria:** What must be complete before starting
- **Exit Criteria:** Deliverable acceptance standards
- **Dependencies:** Upstream tasks that must complete first
- **Estimated Duration:** Time allocation per task

The plan follows OAIS (Open Archival Information System) principles, Dublin Core metadata standards, and PREMIS preservation metadata specifications as outlined in the PRD.

---

## Phases Overview

| Phase | Name | Duration | Key Deliverable |
|-------|------|----------|-----------------|
| 1 | Foundation & Repository Setup | Week 1-2 | Directory structure, vocabulary JSON, category config |
| 2 | Static Site Framework | Week 2-4 | Astro project with RTL Urdu interface |
| 3 | Ingestion Pipeline | Week 3-4 | Automated PDF ingest with metadata creation |
| 4 | Preservation & Integrity | Week 4 | OAIS-compliant fixity checks and backups |
| 5 | Deployment & Infrastructure | Week 5 | Production deployment with CDN |
| 6 | Launch & Content Population | Week 6 | 100 PDFs catalogued and live |

**Critical Path:** 1 → 2 → 3 → 4 → 5 → 6

---

## Detailed Task Breakdown

### PHASE 1: Foundation & Repository Setup

**Duration:** Week 1-2  
**Objective:** Create the physical and logical infrastructure required for the archive.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 1.1 | Create directory structure | 2h | Project start | All 7 category folders exist under `content/` | None |
| 1.2 | Initialize metadata directories | 1h | 1.1 | `metadata/{articles,authors,tags}` exist | 1.1 |
| 1.3 | Create preservation directories | 1h | 1.1 | `checksums/`, `backups/`, `versions/` exist | 1.1 |
| 1.4 | Populate controlled vocabulary JSON | 4h | 1.2 | `metadata/tags/vocabulary.json` matches PRD Section 9 | 1.2 |
| 1.5 | Create category configuration | 2h | 1.1 | `config/categories.json` with all 7 categories and codes | 1.1 |
| 1.6 | Initialize Git repository | 30m | Project start | Initial commit with folder structure | None |
| 1.7 | Setup development environment | 4h | 1.6 | Node.js 18+, Bash, Ghostscript, pdfinfo installed | 1.6 |

**Phase 1 Acceptance Criteria:**
- [ ] Directory tree matches PRD Section 20 (File Naming Standard)
- [ ] `vocabulary.json` contains all required tags (عبادات, معاملات, عقائد, سیرت) with child tags
- [ ] Category codes map correctly (AQEEDAH → عقیدہ, FIQH → فقہ, HADITH → حدیث, TAFSIR → تفسیر, SEERAH → سیرت, RADD → ردّ باطل فرق, MASAIL → عصری مسائل)
- [ ] Git repository initialized with `.gitignore` for Node.js/Astro

---

### PHASE 2: Static Site Framework

**Duration:** Week 2-4  
**Objective:** Build Astro project with RTL Urdu interface, mobile-first design.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 2.1 | Initialize Astro project | 2h | Phase 1 complete | `package.json`, `astro.config.mjs` exist with SSR disabled (static output) | Phase 1 |
| 2.2 | Configure Tailwind CSS with RTL | 2h | 2.1 | `tailwind.config.mjs` with RTL support, custom Urdu font config | 2.1 |
| 2.3 | Setup Urdu/Arabic fonts | 2h | 2.2 | Noto Nastaliq Urdu + Noto Sans Arabic loaded via font-face | 2.2 |
| 2.4 | Create base layout with RTL | 3h | 2.3 | `layouts/BaseLayout.astro` with `dir="rtl"`, lang="ur" | 2.3 |
| 2.5 | Build homepage template | 4h | 2.4 | Homepage displays with Urdu navigation (کتب, تصنیف, حجت, سیرت, موضوعات, فہرست) | 2.4 |
| 2.6 | Create category index pages | 4h | 2.5 | 7 category pages (`/category/aqeedah`, etc.) listing articles | 2.5 |
| 2.7 | Build article detail template | 6h | 2.5 | `/article/[slug]` with all required fields per PRD Section 10 | 2.5 |
| 2.8 | Create author page template | 4h | 2.5 | `/author/[id]` showing author works with authority record info | 2.5 |
| 2.9 | Build tag/topic page template | 4h | 2.5 | `/topic/[tag]` filtering articles by controlled vocabulary | 2.5 |
| 2.10 | Implement search page | 6h | 2.5 | `/search` with Fuse.js integration, searching Title/Tags/Author/Category | 2.5 |
| 2.11 | Create catalog/index pages | 4h | 2.5 | `/catalog`, `/popular`, `/new` pages per PRD Section 5 | 2.5 |
| 2.12 | Build master-index (علمی فہرست) | 3h | 2.5 | Traditional catalog style page with hierarchical listing | 2.5 |
| 2.13 | Implement related articles | 3h | 2.7 | Display minimum 3 related articles (same tags > same author > same category) | 2.7 |
| 2.14 | Add mobile-first styling | 4h | 2.5-2.12 | Large download button, minimal scrolling, clear typography per PRD Section 29 | 2.5-2.12 |

**Phase 2 Acceptance Criteria:**
- [ ] All pages render in RTL mode with proper text direction
- [ ] Navigation shows Urdu labels as specified (کتب, تصنیف, حجت, سیرت, موضوعات, فہرست)
- [ ] Article page displays: title, author (linked), category, tags, pages, file size, synopsis, download button, related articles
- [ ] Mobile-first responsive design verified on actual devices (iOS Safari, Android Chrome)
- [ ] Page load time < 2 seconds (PRD Section 28)

---

### PHASE 3: Ingestion Pipeline

**Duration:** Week 3-4  
**Objective:** Automated PDF ingest with metadata creation, duplicate detection, and search indexing.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 3.1 | Build checksum generation script | 2h | Phase 1 | `scripts/generate-checksum.sh` produces SHA-256 for any file | Phase 1 |
| 3.2 | Create PDF metadata extractor | 3h | 3.1 | `scripts/extract-metadata.sh` extracts page count, file size, PDF version using pdfinfo | 3.1 |
| 3.3 | Implement duplicate detection | 4h | 3.1 | Script checks existing checksums before ingest (per PRD Section 17) | 3.1 |
| 3.4 | Build unique ID generator | 3h | 3.3 | Auto-increment logic per category (CAT-001, CAT-002, etc.) | 3.3 |
| 3.5 | Create watermark script | 4h | 3.2 | `scripts/watermark-pdf.sh` adds library name + website on every page (PRD Section 22) | 3.2 |
| 3.6 | Build main ingest script | 6h | 3.4, 3.5 | `scripts/ingest-pdf.sh` orchestrates full pipeline per PRD Section 31 | 3.4, 3.5 |
| 3.7 | Create metadata JSON generator | 4h | 3.4 | Produces valid article JSON per PRD Sections 6, 20 | 3.4 |
| 3.8 | Build search index rebuild script | 3h | 3.7 | `scripts/build-search-index.js` creates Fuse.js index from all metadata | 3.7 |
| 3.9 | Create sitemap generator | 2h | 3.8 | `scripts/generate-sitemap.sh` produces XML sitemap per PRD Section 11 | 3.8 |
| 3.10 | Implement version control logic | 3h | 3.5 | Copies to `versions/{id}/v{major}.{minor}.pdf` (PRD Section 13) | 3.5 |
| 3.11 | Create author authority record creator | 3h | Phase 1 | `scripts/create-author.sh` makes AUTH-### records (PRD Section 7) | Phase 1 |
| 3.12 | Test full ingest workflow | 4h | 3.6-3.11 | End-to-end test with sample PDF, verify all outputs | 3.6-3.11 |

**Phase 3 Acceptance Criteria:**
- [ ] `ingest-pdf.sh` completes full workflow without manual intervention
- [ ] Generated metadata JSON passes schema validation against PRD Section 6
- [ ] Checksums stored in `checksums/` and metadata, verifiable
- [ ] Watermarked PDF retains text layer (not scanned images per PRD Section 16)
- [ ] Search index rebuilds automatically after each ingest
- [ ] Sitemap includes all article pages with proper URLs
- [ ] Duplicate detection blocks publication of identical checksums

---

### PHASE 4: Preservation & Integrity

**Duration:** Week 4  
**Objective:** OAIS-compliant preservation with fixity verification and disaster recovery.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 4.1 | Build fixity check script | 3h | Phase 3 | `scripts/verify-checksums.sh` compares stored vs computed SHA-256 | Phase 3 |
| 4.2 | Create preservation metadata updater | 2h | 4.1 | Updates `lastFixityCheck`, `fixityStatus`, logs all actions | 4.1 |
| 4.3 | Implement backup script | 4h | 4.1 | `scripts/backup.sh` creates dated `archive-package/` structure per PRD Section 24 | 4.1 |
| 4.4 | Setup incremental backup schedule | 2h | 4.3 | Cron configuration for daily incremental, weekly full (PRD Section 24) | 4.3 |
| 4.5 | Build health monitoring dashboard | 4h | 4.2 | JSON metrics endpoint: fixity pass rate, backup success, metadata completeness | 4.2 |
| 4.6 | Create disaster recovery documentation | 3h | 4.4 | `docs/DISASTER-RECOVERY.md` with 7-step recovery procedure (PRD Section 24) | 4.4 |
| 4.7 | Implement format migration policy | 3h | Phase 3 | Logic to store original + migrated version per PRD Section 15 | Phase 3 |
| 4.8 | Test fixity verification | 2h | 4.1 | Verify all checksums match for test set; detect simulated corruption | 4.1 |
| 4.9 | Validate backup restoration | 3h | 4.3, 4.6 | Test full restore from backup package, verify functionality | 4.3, 4.6 |

**Phase 4 Acceptance Criteria:**
- [ ] Fixity check detects any file corruption with 100% accuracy
- [ ] Backup creates valid `archive-package/` structure with manifest.json
- [ ] Health metrics JSON updates on schedule, accessible via endpoint
- [ ] Disaster recovery document is complete and tested with actual restore
- [ ] Original files never modified — new versions stored separately (PRD Section 13)

---

### PHASE 5: Deployment & Infrastructure

**Duration:** Week 5  
**Objective:** Production deployment with CDN, security headers, and CI/CD pipeline.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 5.1 | Configure hosting provider | 3h | Phase 2 | Cloudflare Pages / Vercel project created with static build | Phase 2 |
| 5.2 | Setup CDN caching rules | 2h | 5.1 | `/content/` PDFs cached with 1-year TTL, articles with 1-hour TTL | 5.1 |
| 5.3 | Configure security headers | 2h | 5.1 | X-Content-Type-Options, X-Frame-Options, Referrer-Policy set (PRD Section 23) | 5.1 |
| 5.4 | Setup custom domain | 2h | 5.2 | Domain DNS configured with CNAME to hosting provider | 5.2 |
| 5.5 | Configure build pipeline | 3h | 5.1 | Auto-deploy on git push to main branch | 5.1 |
| 5.6 | Setup environment variables | 1h | 5.5 | API keys, storage credentials configured securely | 5.5 |
| 5.7 | Test production build | 2h | 5.6 | `npm run build` succeeds without errors, generates `dist/` | 5.6 |
| 5.8 | Verify CDN serving | 2h | 5.4 | PDFs load from CDN URL with correct cache headers | 5.4 |
| 5.9 | Implement download abuse prevention | 3h | 5.3 | Rate limiting or similar protection per PRD Section 23 | 5.3 |
| 5.10 | Load testing | 3h | 5.8 | Verify traffic spike handling per PRD Section 28 | 5.8 |

**Phase 5 Acceptance Criteria:**
- [ ] Site builds and deploys successfully on every push
- [ ] PDFs served via CDN with correct headers (Content-Type: application/pdf)
- [ ] Custom domain resolves correctly with HTTPS
- [ ] Build pipeline triggers on commit, completes in < 5 minutes
- [ ] Download start < 1 second (PRD Section 28)
- [ ] Security headers present on all responses

---

### PHASE 6: Launch & Content Population

**Duration:** Week 6  
**Objective:** Populate content, final testing, and public launch.

| # | Task | Duration | Entrances | Exit Criteria | Dependencies |
|---|------|----------|-----------|---------------|--------------|
| 6.1 | Ingest initial 100 PDFs | 8h | Phase 3, Phase 4 | All PDFs catalogued with complete metadata per PRD Section 30 | Phase 3, Phase 4 |
| 6.2 | Create author authority records | 4h | Phase 3 | All AUTH-### records populated with canonical names, alternate spellings | Phase 3 |
| 6.3 | Verify related articles logic | 2h | Phase 2 | Each article has ≥3 related when available (PRD Section 18) | Phase 2 |
| 6.4 | Test search accuracy | 3h | Phase 2, 3 | >90% relevance on test queries (Title, Tags, Author, Category) | Phase 2, 3 |
| 6.5 | Verify mobile experience | 2h | Phase 2 | Download button prominent, minimal scrolling, fast loading | Phase 2 |
| 6.6 | Performance testing | 2h | Phase 5 | Page load <2s, download start <1s on 3G connection | Phase 5 |
| 6.7 | Submit sitemap to search engines | 1h | Phase 3 | Google Search Console verified, sitemap submitted | Phase 3 |
| 6.8 | Verify metadata completeness | 2h | 6.1 | 100% of articles have all required fields (PRD Section 6) | 6.1 |
| 6.9 | Final pre-launch checklist | 2h | 6.1-6.8 | All acceptance criteria pass, no critical bugs | 6.1-6.8 |
| 6.10 | Launch announcement | 2h | 6.9 | Published to forums/WhatsApp per PRD Section 3 | 6.9 |
| 6.11 | Post-launch monitoring | Ongoing | 6.10 | Monitor metrics for 30 days, health dashboard green | 6.10 |

**Phase 6 Acceptance Criteria:**
- [ ] 100 PDFs ingested with complete metadata per PRD requirements
- [ ] All index pages generated and accessible (All Articles, All Authors, All Topics, Most Downloaded, Recently Added, علمی فہرست)
- [ ] Search returns relevant results with >90% accuracy
- [ ] Mobile responsive verified on iOS and Android devices
- [ ] Download works on mobile (iOS Safari, Android Chrome)
- [ ] Fixity pass rate: 100% (PRD Section 26)
- [ ] Backup validation success rate: 100%
- [ ] Metadata completeness rate: 100%

---

## Dependency Graph

```
PROJECT START
    │
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 1: Foundation                                 │
│ 1.1 → 1.2, 1.3, 1.5 (parallel after 1.1)            │
│ 1.4 depends on 1.2                                  │
│ 1.6 independent, 1.7 depends on 1.6                 │
└─────────────────────────────────────────────────────┘
    │ All Phase 1 exit criteria met
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 2: Static Site                                │
│ 2.1 → 2.2 → 2.3 → 2.4 → 2.5                        │
│ 2.5 branches to: 2.6, 2.7, 2.8, 2.9, 2.10, 2.11,    │
│                  2.12 (parallel after 2.5)          │
│ 2.13 depends on 2.7                                 │
│ 2.14 depends on 2.5-2.13                            │
└─────────────────────────────────────────────────────┘
    │ All Phase 2 exit criteria met
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 3: Ingestion Pipeline                         │
│ 3.1 → 3.2 → 3.5                                    │
│ 3.1 → 3.3 → 3.4 → 3.6, 3.7, 3.10                   │
│ 3.6, 3.7, 3.10 converge at 3.8, 3.9                │
│ 3.11 independent (can run with Phase 1)             │
│ 3.12 tests all 3.x                                  │
└─────────────────────────────────────────────────────┘
    │ All Phase 3 exit criteria met
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 4: Preservation & Integrity                  │
│ 4.1 → 4.2 → 4.5                                    │
│ 4.1 → 4.3 → 4.4 → 4.6, 4.9                         │
│ 4.7 independent (can run with Phase 3)              │
│ 4.8 tests 4.1                                       │
└─────────────────────────────────────────────────────┘
    │ All Phase 4 exit criteria met
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 5: Deployment                                  │
│ 5.1 → 5.2 → 5.4                                     │
│ 5.1 → 5.3 → 5.9                                     │
│ 5.1 → 5.5 → 5.6 → 5.7                               │
│ 5.4, 5.9, 5.7 converge at 5.8, 5.10                │
└─────────────────────────────────────────────────────┘
    │ All Phase 5 exit criteria met
    ▼
┌─────────────────────────────────────────────────────┐
│ PHASE 6: Launch                                      │
│ 6.1, 6.2 (content population - parallel)            │
│ 6.3-6.8 (testing - sequential after 6.1, 6.2)       │
│ 6.9 (go/no-go decision)                             │
│ 6.10 (launch) → 6.11 (monitoring)                   │
└─────────────────────────────────────────────────────┘
    │
    ▼
  COMPLETE
```

---

## Files to Create/Modify

### New Files to Create (Configuration)

| Path | Purpose | Phase |
|------|---------|-------|
| `package.json` | Node.js dependencies: astro, @astrojs/tailwind, fuse.js | 2.1 |
| `astro.config.mjs` | Astro configuration: static output, build options | 2.1 |
| `tailwind.config.mjs` | Tailwind with RTL support, Urdu font family | 2.2 |
| `tsconfig.json` | TypeScript configuration | 2.1 |
| `config/categories.json` | Category definitions with codes and Urdu labels | 1.5 |
| `.gitignore` | Node.js, Astro, OS-specific ignores | 1.6 |

### New Files to Create (Source Code)

| Path | Purpose | Phase |
|------|---------|-------|
| `src/layouts/BaseLayout.astro` | RTL base layout with Urdu fonts | 2.4 |
| `src/pages/index.astro` | Homepage with navigation | 2.5 |
| `src/pages/catalog.astro` | Full catalog of all articles | 2.11 |
| `src/pages/category/[code].astro` | Dynamic category pages (7 categories) | 2.6 |
| `src/pages/article/[slug].astro` | Article detail page with all metadata | 2.7 |
| `src/pages/author/[id].astro` | Author page with authority record | 2.8 |
| `src/pages/topic/[tag].astro` | Tag/topic filtering page | 2.9 |
| `src/pages/search.astro` | Search page with Fuse.js | 2.10 |
| `src/pages/popular.astro` | Most downloaded articles | 2.11 |
| `src/pages/new.astro` | Recently added articles | 2.11 |
| `src/pages/master-index.astro` | Classical-style catalog (علمی فہرست) | 2.12 |
| `src/components/Header.astro` | Site header with navigation | 2.4 |
| `src/components/Footer.astro` | Site footer | 2.4 |
| `src/components/ArticleCard.astro` | Article preview card | 2.5 |
| `src/components/AuthorCard.astro` | Author preview card | 2.8 |
| `src/components/TagBadge.astro` | Tag display component | 2.5 |
| `src/components/DownloadButton.astro` | Prominent download button | 2.7 |
| `src/components/RelatedArticles.astro` | Related articles section | 2.13 |
| `src/components/SearchBox.astro` | Search input component | 2.10 |
| `src/lib/metadata.ts` | Metadata loading utilities | 2.1 |
| `src/lib/search.ts` | Fuse.js search logic | 2.10 |
| `src/lib/utils.ts` | Utility functions (slugify, formatDate, etc.) | 2.1 |
| `src/styles/global.css` | Global styles with Urdu typography | 2.3 |

### New Files to Create (Scripts)

| Path | Purpose | Phase |
|------|---------|-------|
| `scripts/ingest-pdf.sh` | Main ingestion pipeline orchestrator | 3.6 |
| `scripts/generate-checksum.sh` | SHA-256 generation for any file | 3.1 |
| `scripts/extract-metadata.sh` | PDF info extraction (pages, size, version) | 3.2 |
| `scripts/watermark-pdf.sh` | Add library watermark to PDF | 3.5 |
| `scripts/build-search-index.js` | Build Fuse.js search index | 3.8 |
| `scripts/generate-sitemap.sh` | Generate XML sitemap | 3.9 |
| `scripts/verify-checksums.sh` | Fixity verification for all files | 4.1 |
| `scripts/backup.sh` | Create archive package backup | 4.3 |
| `scripts/create-author.sh` | Create author authority record | 3.11 |
| `scripts/restore-from-backup.sh` | Disaster recovery restoration | 4.6 |

### New Files to Create (Data/Content)

| Path | Purpose | Phase |
|------|---------|-------|
| `metadata/tags/vocabulary.json` | Controlled vocabulary (PRD Section 9) | 1.4 |
| `metadata/authors/` | Directory for AUTH-###.json files | 1.2 |
| `metadata/articles/` | Directory for article metadata JSON | 3.7 |
| `checksums/` | Directory for .sha256 files | 1.3 |
| `backups/` | Directory for archive packages | 1.3 |
| `versions/` | Directory for versioned PDFs | 1.3 |
| `content/aqeedah/` | Creed category PDFs | 1.1 |
| `content/fiqh/` | Jurisprudence category PDFs | 1.1 |
| `content/hadith/` | Hadith category PDFs | 1.1 |
| `content/tafsir/` | Exegesis category PDFs | 1.1 |
| `content/seerah/` | Biography category PDFs | 1.1 |
| `content/radd/` | Refutations category PDFs | 1.1 |
| `content/masail/` | Contemporary Issues PDFs | 1.1 |

### New Files to Create (Documentation)

| Path | Purpose | Phase |
|------|---------|-------|
| `docs/IMPLEMENTATION-PLAN.md` | This document | — |
| `docs/DISASTER-RECOVERY.md` | 7-step recovery procedure | 4.6 |
| `docs/USER-GUIDE.md` | Usage documentation for administrators | 6.9 |
| `docs/INGEST-WORKFLOW.md` | Detailed ingest process documentation | 3.12 |
| `docs/METADATA-SCHEMA.md` | JSON schema documentation | 3.7 |

---

## Risk Assessment

### Technical Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| PDF text layer missing | High (accessibility failure) | Medium | Pre-ingest validation: reject PDFs without text layer using pdfinfo |
| Urdu font rendering issues | Medium (UX degradation) | Medium | Test across devices; provide fallback font stack; use system fonts as fallback |
| Large file handling | Medium (performance) | Low | CDN with aggressive caching; lazy load non-critical resources |
| Checksum false positives | Low (integrity alerts) | Low | Run fixity on subset first; manual verification before alerts |
| Search relevance poor | Medium (discoverability) | Medium | Tune Fuse.js thresholds; add field weights; user testing |
| Watermark breaks text layer | High (accessibility) | Medium | Test watermark script on sample PDFs; verify text remains selectable |

### Operational Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Metadata inconsistency | High (search/catalog broken) | Medium | JSON Schema validation in ingest pipeline; automated tests |
| Broken URLs after migration | Medium (SEO loss) | Low | 301 redirects mandatory; URL permanence enforced (PRD Section 11) |
| Backup corruption | High (data loss) | Low | Test restore quarterly; multiple storage locations; verify checksums |
| Author name variations | Medium (duplicate authors) | Medium | Authority record system; manual review of new authors |
| Tag vocabulary drift | Medium (inconsistency) | Low | Strict controlled vocabulary; approval process for new tags |

### OAIS Compliance Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Original files modified | High (preservation failure) | Immutable storage; version control; audit logs |
| Fixity checks skipped | Medium (undetected corruption) | Automated scheduling; alerting; health dashboard |
| Metadata not portable | Medium (lock-in) | Export functions; standard formats (Dublin Core, PREMIS) |
| Preservation actions not logged | Medium (audit failure) | Automated logging; versionHistory field updates |

---

## Success Metrics

| Metric | Target | Measurement Method | Phase |
|--------|--------|-------------------|-------|
| PDFs catalogued | 100 | Count metadata records | 6 |
| Page load time | < 2 seconds | Lighthouse / WebPageTest | 2, 6 |
| Download start time | < 1 second | Network tab timing | 5, 6 |
| Search accuracy | > 90% | Manual test with 20 queries | 6 |
| Fixity pass rate | 100% | Monthly verification report | 4+ |
| Backup success rate | 100% | Weekly validation report | 4+ |
| Metadata completeness | 100% | Automated validation | 3, 6 |
| Mobile usability | Pass | Google Mobile-Friendly Test | 2, 6 |
| URL permanence | 100% | No 404s for published articles | 6 |
| Content accessibility | 100% | All PDFs have text layer | 3, 6 |

---

## PRD Compliance Checklist

| PRD Section | Implementation Coverage | Verification |
|-------------|------------------------|--------------|
| 1. Product Vision | Full | RTL Urdu interface, mobile-first |
| 2. Core Objectives | Full | All 5 objectives addressed |
| 3. Target Users | Full | User journey implemented |
| 4. Content Structure | Full | Metadata + PDF separation |
| 5. Information Architecture | Full | 7 categories, tag hierarchy, index pages |
| 6. Metadata Model | Full | All required fields implemented |
| 7. Author Authority Records | Full | AUTH-### system |
| 8. Metadata Normalization | Full | Validation rules enforced |
| 9. Controlled Vocabulary | Full | vocabulary.json with hierarchy |
| 10. Article Page Layout | Full | All required elements present |
| 11. URL Structure | Full | /category/article-slug format |
| 12. Download Behavior | Full | Direct download, no preview |
| 13. PDF Version Policy | Full | v{major}.{minor} versioning |
| 14. File Fixity Checks | Full | Monthly/quarterly schedule |
| 15. Format Migration | Full | Original retained, migrated alongside |
| 16. PDF Accessibility | Full | Text layer validation |
| 17. Duplicate Detection | Full | Checksum, title, slug checks |
| 18. Related Articles | Full | Tag > Author > Category priority |
| 19. Search System | Full | Fuse.js with index refresh |
| 20. Content Organization | Full | CAT-### ID format |
| 21. Content Acceptance | Full | Validation in ingest pipeline |
| 22. Rights & Distribution | Full | Watermark on every page |
| 23. Security Requirements | Full | Headers, encryption, bot protection |
| 24. Backup & Disaster Recovery | Full | Daily incremental, weekly full |
| 25. Retention Policy | Full | Archived state handling |
| 26. Health Monitoring | Full | Dashboard with all metrics |
| 27. Analytics & Privacy | Full | Aggregate only, no personal data |
| 28. Performance Targets | Full | <2s page, <1s download |
| 29. Mobile Experience | Full | Large button, minimal scroll |
| 30. Scalability | Full | 100 → 250 → 500+ support |
| 31. Content Workflow | Full | Automated pipeline |
| 32. Success Metrics | Full | Tracking implemented |
| 33. Product Principles | Full | All 8 principles embodied |
| 34. Compliance Reference | Full | OAIS, Dublin Core, PREMIS |

---

## Conclusion

This implementation plan provides a complete breakdown of the Digital Islamic PDF Library build with **47 discrete tasks** organized into **6 sequential phases**, each with clear entrance/exit criteria and dependency ordering. The plan ensures OAIS compliance, mobile-first design, and long-term archival stability while maintaining the scholarly organization requirements specified in the PRD.

**Next Steps:**
1. Review and approve this implementation plan
2. Begin Phase 1 tasks in order
3. Track progress against exit criteria
4. Conduct phase-gate reviews before proceeding to next phase

---

*Document Version: 1.0*  
*Created: Based on PRD Version 5.0*  
*Status: Implementation Ready*
