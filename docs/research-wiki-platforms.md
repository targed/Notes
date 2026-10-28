# Technical Research Report: Evaluation of Self-Hostable Wiki & Documentation Platforms for Logseq Markdown Notes

## Executive Summary

This investigation evaluates eight candidate documentation and wiki platforms (Docmost, AFFiNE, Outline, BookStack, XWiki, Wiki.js, AppFlowy, Anytype) for hosting, searching, and navigating a repository of university course lecture notes created in Logseq. The repository contains 979 Markdown files across 19 course directories, featuring `[[wikilinks]]`, inline and block LaTeX math (`$...$` and `$$...$$`), and hierarchical outliner bullets.

The primary finding is an **architectural mismatch** between the user's operational requirement ("preferably through GitHub Actions") and the architectural nature of all eight candidate platforms:
- **All eight listed platforms are full-stack server applications or client-side sync daemons.** None of them can be compiled statically and served from GitHub Actions or GitHub Pages. Each requires running a continuous 24/7 host (VPS, container stack, or homelab) with relational databases (PostgreSQL, MySQL), caches (Redis), and background daemons.
- **Logseq conventions break on traditional wikis.** Standard wikis either reject unpathed `[[Page Name]]` links, require proprietary document UUIDs, or enforce rigid categorization (e.g., BookStack's Shelves-Books-Chapters-Pages) that conflicts with arbitrary directory graphs.
- **Git-native static graph generators (Quartz v4, Logseq Native Publish, MkDocs Material)** exist specifically to solve this exact problem: they build in seconds within GitHub Actions, deploy for free to GitHub Pages with zero server maintenance, and natively resolve Logseq wikilinks, math, outlines, and interactive graph views.

---

## 1. Evaluation of the 8 Candidate Platforms

### 1.1 Docmost
* **Primary Sources**: [Docmost Documentation](https://docmost.com/docs), [Docmost GitHub](https://github.com/docmost/docmost)
* **Architecture & Storage**: Docmost is a collaborative wiki built on NestJS (Node.js), React, and Tiptap. Document state is stored as JSON/ProseMirror structures in PostgreSQL with Redis handling caching and real-time collaboration. File attachments live on local disk or S3.
* **Git Sync & Headless Ingestion**:
  * No built-in Git synchronization module.
  * Markdown import is supported via UI drag-and-drop or ZIP archive upload.
  * Docmost provides a REST API, but there is no official GitHub Action. Syncing via GitHub Actions requires writing a custom Python/Node script to call Docmost API endpoints.
* **Logseq Conventions**:
  * *Wikilinks*: Uses internal `@` mentions and page links. Raw `[[Page Name]]` syntax does not resolve automatically to other files across nested directories.
  * *LaTeX*: Supported via KaTeX for inline and block expressions.
  * *Outliner*: Rendered as standard rich-text bullet lists, lacking outliner-style folding or block zooming.
  * *Graph View*: No knowledge graph visualization.
* **Hosting Overhead**: Requires a running Docker Compose environment (Docmost app container, PostgreSQL 16+, Redis 7+). Cannot be served from GitHub Actions or GitHub Pages.

---

### 1.2 AFFiNE
* **Primary Sources**: [AFFiNE Documentation](https://affine.pro/docs), [AFFiNE GitHub](https://github.com/toeverything/AFFiNE)
* **Architecture & Storage**: Built on Rust and Node.js using BlockSuite. Workspaces are stored as Conflict-free Replicated Data Types (CRDTs via `Yjs`) backed by SQLite locally or PostgreSQL and Redis in self-hosted Docker deployments.
* **Git Sync & Headless Ingestion**:
  * Not a flat-file Markdown system. Content is serialized into binary CRDT block stores.
  * No headless Git synchronization daemon or official GitHub Action.
  * Markdown files can be imported manually through the UI, but continuous synchronization from a Git repository requires complex, non-standard scripts converting Markdown to BlockSuite snapshot data.
* **Logseq Conventions**:
  * *Wikilinks*: Page linking is supported, but internal pointers depend on block UUIDs rather than file paths.
  * *LaTeX*: Supported in math blocks and inline formulas.
  * *Outliner*: Block-based editing allows indented blocks, but behaves like Notion/Miro rather than a Roam/Logseq outliner.
  * *Graph View*: No automatic knowledge network graph. Visual organization is done manually via "Edgeless" infinite whiteboard canvas mode.
* **Hosting Overhead**: Docker Compose stack requiring the AFFiNE server, PostgreSQL, Redis, and blob storage. Cannot run statically.

---

### 1.3 Outline
* **Primary Sources**: [Outline Documentation](https://docs.getoutline.com), [Outline GitHub](https://github.com/outline/outline)
* **Architecture & Storage**: Built with Node.js, React, and ProseMirror. Documents are stored in PostgreSQL as structured text. File uploads require an S3-compatible object store (e.g., AWS S3, MinIO, Cloudflare R2).
* **Git Sync & Headless Ingestion**:
  * Outline provides a comprehensive RPC-style HTTP REST API (`/api/documents.create`, `/api/documents.update`).
  * Community GitHub Actions exist (such as `benhowes/outline-sync-action`) to push Markdown files into specific Outline collections on commit.
* **Logseq Conventions**:
  * *Wikilinks*: Outline uses internal UUIDs for document links. When importing raw `[[Page Name]]` wikilinks, Outline does not automatically resolve them to the target document ID unless a custom preprocessor maps titles to UUIDs.
  * *LaTeX*: Full native KaTeX math support.
  * *Outliner*: Standard Markdown lists.
  * *Graph View*: None native. Feature requests remain open; Outline focuses on document hierarchy rather than network graphs.
* **Hosting Overhead**:
  * Heavy infrastructure: Node.js app container, PostgreSQL, Redis, MinIO/S3.
  * **Strict SSO Requirement**: Outline does not support basic username/password authentication out of the box; self-hosted instances must configure an external OpenID Connect (OIDC) provider (Keycloak, Authentik, Google, or Slack).
  * Cannot be hosted serverlessly or via GitHub Pages.

---

### 1.4 BookStack
* **Primary Sources**: [BookStack Documentation](https://www.bookstackapp.com/docs), [BookStack GitHub](https://github.com/BookStackApp/BookStack)
* **Architecture & Storage**: Built on PHP (Laravel) and MySQL/MariaDB. Pages are saved in MySQL tables as HTML and Markdown.
* **Git Sync & Headless Ingestion**:
  * No built-in Git sync engine.
  * REST API is available. Community tools (e.g., `lazybytez/sync-markdown-bookstack-action` and `Bookstack-MD-Importer`) can map folders to Books and Chapters.
* **Logseq Conventions**:
  * *Hierarchy*: BookStack enforces a strict three-tier hierarchy: `Shelves -> Books -> Chapters -> Pages`. Arbitrary nested directory structures (such as `Course/Lecture/Section/Note.md`) do not fit cleanly without flattening or restructuring.
  * *Wikilinks*: No native `[[Page Name]]` parsing. Links must use BookStack URL structures or standard Markdown `[text](url)` syntax.
  * *LaTeX*: Not supported natively. Requires injecting a custom MathJax script into the application header.
  * *Outliner*: Standard HTML lists.
  * *Graph View*: None.
* **Hosting Overhead**: Requires a PHP runtime and MySQL database (typically via Linuxserver/Bookstack Docker container). Cannot be hosted statically.

---

### 1.5 XWiki
* **Primary Sources**: [XWiki Documentation](https://www.xwiki.org), [XWiki System Requirements](https://www.xwiki.org/xwiki/bin/view/Documentation/AdminGuide/Performances/)
* **Architecture & Storage**: Enterprise-grade Java application running on Tomcat or Jetty, backed by a relational database (PostgreSQL/MySQL) and Solr search engine.
* **Git Sync & Headless Ingestion**:
  * Includes a "GitHub Importer Application" extension that can clone GitHub wikis and convert Markdown into XWiki syntax.
  * REST API allows programmatic document creation.
* **Logseq Conventions**:
  * *Wikilinks*: Uses XWiki syntax (`[[Label>>doc:Space.Page]]`), differing from standard Logseq bracket syntax.
  * *LaTeX*: Handled via macros (`{{mathjax}}` or `{{formula}}`).
  * *Outliner*: Wiki-syntax bullet lists.
  * *Graph View*: Not available out of the box.
* **Hosting Overhead**: Extremely resource-heavy. Requires a dedicated Java Virtual Machine (JVM) with a minimum recommended heap of 2 GB to 4 GB of RAM, Tomcat, and a database. Disproportionate overhead for personal class notes.

---

### 1.6 Wiki.js
* **Primary Sources**: [Wiki.js Documentation](https://docs.requarks.com), [Wiki.js Git Storage Guide](https://docs.requarks.com/storage/git)
* **Architecture & Storage**: Node.js wiki engine backed by PostgreSQL (or SQLite/MySQL).
* **Git Sync & Headless Ingestion**:
  * **Native Git Storage Module**: Wiki.js includes built-in two-way synchronization with a remote Git repository via SSH. Changes pushed to GitHub can be automatically pulled and indexed by Wiki.js.
  * *Critical Constraint*: Wiki.js requires the Git repository to be dedicated entirely to Wiki.js. It also expects structured YAML frontmatter (with properties like `title`, `description`, `published`) on every file; files with arbitrary names or special characters (such as colons in lecture titles) can fail to parse.
* **Logseq Conventions**:
  * *Wikilinks*: Supported in 3.x betas, but 2.x primarily uses URL-based paths.
  * *LaTeX*: Built-in KaTeX/MathJax support with `$...$` and `$$...$$`.
  * *Outliner*: Standard Markdown lists.
  * *Graph View*: No graph visualization.
* **Hosting Overhead**: Requires a 24/7 Node.js server and database. Stagnant development (v2 is in maintenance; v3 has experienced prolonged development delays). Cannot be hosted via GitHub Actions.

---

### 1.7 AppFlowy
* **Primary Sources**: [AppFlowy Documentation](https://docs.appflowy.io), [AppFlowy GitHub](https://github.com/AppFlowy-IO/AppFlowy)
* **Architecture & Storage**: Written in Flutter and Rust. Primarily a client-side desktop/mobile application with an optional self-hosted backend (AppFlowy Cloud) utilizing PostgreSQL, GoTrue auth, Redis, and MinIO.
* **Git Sync & Headless Ingestion**:
  * No Git storage engine.
  * No headless CI/CD pipeline for importing a repository of Markdown files into an AppFlowy workspace.
* **Logseq Conventions**:
  * *Wikilinks*: Internal page references only.
  * *LaTeX*: Handled via custom Tex block widgets.
  * *Graph View*: Open feature request; no interactive graph view.
* **Hosting Overhead**: Intended as a personal productivity client, not a public documentation website engine. Self-hosting AppFlowy Cloud requires a multi-container Docker stack.

---

### 1.8 Anytype
* **Primary Sources**: [Anytype Documentation](https://docs.anytype.io), [Anytype Any-Sync GitHub](https://github.com/anyproto/any-sync)
* **Architecture & Storage**: Local-first, decentralized note-taking platform built on the Any-Sync protocol (Go) with CRDTs, MongoDB, and Redis.
* **Git Sync & Headless Ingestion**:
  * Completely incompatible with Git workflows. Data is stored as encrypted objects on an IPFS-inspired decentralized network.
  * No headless GitHub Actions import pipeline.
* **Logseq Conventions**:
  * Objects and relations model, completely separate from plain-text Markdown files.
* **Hosting Overhead**: Web publishing only supports sharing individual standalone objects to `any.org` subdomains via the desktop UI. It does not publish connected multi-page sites via GitHub Actions.

---

## 2. Summary Matrix: The 8 Candidates

| Platform | Stored Format | Native Git Sync? | Logseq `[[Wikilinks]]` | LaTeX Math | Graph View | Can Run on GitHub Pages? | Server Stack Required |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Docmost** | PostgreSQL (JSON) | No (API only) | No (UUID/Mentions) | Yes (KaTeX) | No | No | Node.js + Postgres + Redis |
| **AFFiNE** | PostgreSQL (CRDT) | No | No (Block refs) | Yes | No | No | Rust/Node + Postgres + Redis |
| **Outline** | PostgreSQL | No (API / 3rd party) | No (UUIDs) | Yes (KaTeX) | No | No | Node.js + Postgres + Redis + S3 + OIDC |
| **BookStack** | MySQL (HTML/MD) | No (API / 3rd party) | No (Strict URL) | Hack script | No | No | PHP + MySQL |
| **XWiki** | Relational DB | No (Importer app) | No (XWiki syntax) | Macro only | No | No | Java (Tomcat) + DB + Solr (2-4GB RAM) |
| **Wiki.js** | Relational DB | **Yes** (Git module) | Partial | Yes (KaTeX) | No | No | Node.js + Postgres/SQLite |
| **AppFlowy** | Block DB / Cloud | No | No | Block widget | No | No | Client app + Go/Postgres/MinIO |
| **Anytype** | P2P CRDT objects | No | No | Formula only | Node map | No | Any-Sync (Go + Mongo + Redis) |

---

## 3. The True Git-Native Alternatives

When the objective is to **serve, search, and navigate a Logseq Markdown graph using GitHub Actions**, the industry-standard solutions are static digital garden compilers rather than dynamic wiki servers:

### Option A: Quartz v4 (Recommended)
* **What it is**: An open-source, TypeScript-based static site generator specifically tailored for Obsidian and Logseq knowledge bases.
* **Logseq Compatibility**:
  * Native resolution of `[[wikilinks]]` across arbitrary nested folders without needing relative file paths.
  * Full KaTeX math rendering (`$...$` and `$$...$$`).
  * Fast client-side full-text search (FlexSearch).
  * Interactive global and local graph views.
  * Automatic directory explorer and backlink lists.
* **GitHub Actions Integration**: Built-in GitHub Actions workflow compiles the static site and deploys it to GitHub Pages in ~20 seconds.
* **Cost & Maintenance**: $0/month, zero servers, zero databases.

### Option B: Logseq Native SPA Publisher (`logseq/publish-spa`)
* **What it is**: The official Logseq compiler (`logseq-cli build-graph`) packaged as a GitHub Action.
* **Logseq Compatibility**: 100% exact replica of the Logseq desktop application in the browser (identical UI, full graph view, journals, block zooming, flashcards).
* **GitHub Actions Integration**: Deploys directly to GitHub Pages on every push.
* **Cost & Maintenance**: $0/month, zero servers.

### Option C: MkDocs Material
* **What it is**: Python documentation generator with `mkdocs-roamlinks-plugin` and `pymdownx.arithmatex`.
* **Logseq Compatibility**: Excellent documentation layout, search, and math; handles wikilinks with plugin, but lacks an interactive graph view.
* **GitHub Actions Integration**: Native `mkdocs gh-deploy` pipeline.
* **Cost & Maintenance**: $0/month, zero servers.

---

## 4. Primary Source Citations

1. Docmost Repository & Documentation: https://github.com/docmost/docmost | https://docmost.com/docs
2. AFFiNE Repository & BlockSuite Architecture: https://github.com/toeverything/AFFiNE | https://affine.pro/docs
3. Outline Wiki Repository & REST API Specs: https://github.com/outline/outline | https://docs.getoutline.com
4. BookStack Repository & API Documentation: https://github.com/BookStackApp/BookStack | https://www.bookstackapp.com/docs
5. XWiki Platform Documentation & System Sizing: https://github.com/xwiki/xwiki-platform | https://www.xwiki.org
6. Wiki.js Repository & Git Storage Engine: https://github.com/Requarks/wiki | https://docs.requarks.com/storage/git
7. AppFlowy Repository & Editor Architecture: https://github.com/AppFlowy-IO/AppFlowy | https://docs.appflowy.io
8. Anytype Any-Sync Protocol & Node Setup: https://github.com/anyproto/anytype-heart | https://docs.anytype.io
9. Quartz v4 Digital Garden Compiler: https://github.com/jackyzha0/quartz | https://quartz.jzhao.xyz
10. Logseq Single Page App Publisher Action: https://github.com/logseq/publish-spa
