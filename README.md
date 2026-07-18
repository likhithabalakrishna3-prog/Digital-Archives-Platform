# Archival Collections Demo
### A demonstration of digital archival practice by Likhitha Balakrishna

## About This Repository
This repository demonstrates practical understanding of digital archival
standards, metadata schemas, and open-source archival workflows.

It is structured as a working prototype of a public-facing archival
collections interface — the kind of system used by research institutions,
universities, and cultural heritage organisations to make their collections
discoverable online.

---

## What This Demonstrates

### Archival Standards
- **EAD XML (Encoded Archival Description)** — international standard for
  encoding finding aids, maintained by the Library of Congress. Implemented
  at collection, series and file level with proper EAD 2002 hierarchy.
- **Dublin Core Metadata Schema** — foundational metadata standard with 15
  core elements applied across all collections.
- **OAI-PMH (Open Archives Initiative Protocol for Metadata Harvesting)** —
  standard protocol for harvesting metadata from archival repositories.
  Collections in this demo are sourced directly from live OAI-PMH endpoints.
- **Finding Aids** — structured documents describing the scope, content,
  arrangement and access conditions of archival collections.
- **Metadata Preservation** — all metadata stored in open, non-proprietary
  formats (XML, JSON) to ensure long-term accessibility and interoperability.

### Technical Implementation
- **Dynamic rendering** — HTML reads from a collections index at runtime.
  No collection data is hardcoded. Adding a new collection requires one
  XML file and one line in the index.
- **Dual source support** — the application handles both JSON metadata
  files and EAD XML files through separate loaders that normalise data
  into a consistent internal structure before rendering.
- **OAI-PMH and plain EAD parsing** — the XML parser handles both plain
  EAD exports and OAI-PMH wrapped EAD using namespace-agnostic traversal,
  making it compatible with any valid EAD file regardless of structure.
- **Flexible EAD structure handling** — automatically detects and handles
  both hierarchical (c01 > c02) and flat (c level="file") EAD structures.
- **Hash-based URL routing** — every collection has its own shareable,
  bookmarkable URL. Browser back and forward navigation works correctly.
- **Separated concerns** — HTML for structure, CSS for presentation,
  JavaScript for data and logic. All collection data lives in source files,
  never in the application code.
- **Defensive XML parsing** — pre-processing handles common data quality
  issues in real-world archival exports such as unescaped ampersands.

### Tools & Standards
- EAD XML (EAD 2002)
- Dublin Core Metadata Schema 1.1
- OAI-PMH 2.0
- JSON structured metadata
- Git version control
- GitHub Pages (static hosting)
- JavaScript DOMParser API
- HTML5 / CSS3
- Explored local deployment of ArchivesSpace using Docker and WSL2,
  and consumed live archival data from ArchivesSpace OAI-PMH endpoints.

---

## Repository Structure
archival-collections-demo/
├── index.html                    ← application entry point
├── styles.css                    ← all visual styles
├── generate_ead.py               ← generates EAD XML from JSON metadata
├── webhook_server.py             ← real-time ArchivesSpace sync via webhooks
├── README.md
├── metadata/
│   ├── collections.json          ← master index of all collections
│   └── collection-metadata.json  ← sample Dublin Core JSON collection
└── ead-xml/
└── *.xml                     ← EAD XML finding aids

## Architecture

### Single Source of Truth
All collection data flows from source files — JSON or EAD XML. The
application never stores collection data in code.
collections.json (master index)
↓
collection-metadata.json OR ead-xml/*.xml (source files)
↓
JavaScript loaders (normalise to consistent structure)
↓
Single renderer (displays any collection identically)
↓
Public interface

### Adding a New Collection
1. Export EAD XML from ArchivesSpace (or any EAD-compliant system)
2. Commit the XML file to `ead-xml/`
3. Add one entry to `metadata/collections.json`
4. The collection appears in the interface automatically

---

## Archival Standards

This project explores internationally recognised archival standards
commonly used by archival institutions worldwide:

- **Dublin Core** — A foundational metadata standard used for describing
  digital and physical archival resources. Applied here with all 15 core
  elements including title, creator, subject, description, date, format,
  language and rights.

- **EAD (Encoded Archival Description)** — An XML-based standard maintained
  by the Library of Congress for encoding finding aids. The EAD files in
  this repository follow EAD 2002 structure with proper hierarchy:
  collection → series → file level description, including archdesc, dsc,
  and component level elements.

- **Finding Aids** — A finding aid is the primary access tool for any
  archival collection. This repository includes structured finding aids
  describing the scope, content, arrangement and access conditions of
  each collection — following standard archival practice for public
  collecting institutions.

- **Metadata Preservation** — All metadata is stored in open,
  non-proprietary formats (XML, JSON) to ensure long-term accessibility
  and interoperability with archival systems including ArchivesSpace,
  DSpace and Omeka.

---

## Sync Architecture

This demo uses static XML files committed to the repository. In a
production environment, collections would sync automatically from
ArchivesSpace using one of two approaches:

**Scheduled harvesting** — A nightly GitHub Action fetches the latest
EAD from each OAI-PMH endpoint, compares it with the current version
in Git, and commits any changes automatically. The Git history records
every update with timestamp and provenance.

**Webhook-based real-time sync** — ArchivesSpace fires a webhook
notification whenever a record changes. A lightweight server receives
the notification, fetches the updated EAD from the OAI endpoint, and
commits it to Git immediately. This approach is implemented in
`webhook_server.py`.

In both approaches Git serves as the provenance layer — every change
to every finding aid is tracked, attributed and reversible.

---

## Git as Archival Provenance

Archival documentation — finding aids, EAD XML, metadata records — are
text files. Git is designed for tracking changes to text files.

Every commit is a provenance record: who changed the finding aid, when,
and why. Every diff shows exactly what changed line by line. Every
previous version is recoverable.

This maps directly onto core archival principles:
- **Provenance** — every change is attributed to a person and timestamped
- **Authenticity** — SHA hashes provide cryptographic proof of integrity
- **Non-destruction** — Git never loses history; all previous states
  are recoverable

Institutions such as the Bentley Historical Library at the University
of Michigan use Git-based workflows for managing finding aids — treating
EAD files as code, reviewing changes through pull requests, and
maintaining a full audit trail of every archival description decision.

---

## Note

This project is an independent exploratory portfolio project created
to study digital archival workflows, metadata standards, and
public-facing archival interfaces. It uses real EAD data sourced from
publicly accessible OAI-PMH endpoints for demonstration purposes.

---


## Live Demo 
https://likhithabalakrishna3-prog.github.io/Digital-Archives-Platform/

*Built with open-source archival standards · Likhitha Balakrishna · 2026*
