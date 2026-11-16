# Tableau Technical Documentation Template

## Purpose

This template generates comprehensive technical documentation for Tableau workbooks (.twbx), published data sources (.tds/.tdsx), and Tableau Prep flows (.tfl/.tflx). It is designed for use across any client engagement and produces a consistent, thorough deliverable every time.

---

## Before You Begin — Required Intake

**When this template is used in a new chat, the assistant must collect the following before starting any analysis:**

### 1. Client Information

Prompt the user:

> **Who is this documentation for?**
> Please provide:
> - **Client name** (e.g., "GMH Communities", "Acme Corp")
> - **Prepared by** (team or company name, e.g., "XeoMatrix", "Bulletproof Analytics")

### 2. Files to Document

Prompt the user:

> **Which files need to be documented?**
> Please upload or provide links to each file. Accepted types:
> - `.twbx` — Tableau Packaged Workbook
> - `.twb` — Tableau Workbook (XML only, no data)
> - `.tds` / `.tdsx` — Tableau Data Source
> - `.tfl` / `.tflx` — Tableau Prep Flow
>
> For each file, also provide:
> - **Published URL** (Tableau Cloud/Server link to the live view, if applicable)
> - **Any known refresh schedule or ETL notes** (e.g., "refreshes every 6 hours from 8 AM to 11 PM aligned with BigQuery")
> - **Target audience** for this documentation (developers, analysts, stakeholders, all)

### 3. Output Format

Prompt the user:

> **How would you like the documentation delivered?**
> - Word document (.docx)
> - Google Doc (created in your Google Drive)
> - Markdown (.md)
> - PDF
> - Multiple formats

---

## Analysis Process

Once files and client info are collected, follow this extraction and analysis pipeline for each file type:

### For .twbx / .twb (Workbooks)

1. **Extract the workbook** — Unzip the .twbx to access the .twb XML file
2. **Parse workbook metadata** — Version, revision, platform, locale, site/server info
3. **Catalog data sources** — Connection type, server, tables, relations, column families
4. **Inventory parameters** — Name, data type, default value, domain (list/range/all), purpose
5. **Catalog calculated fields** — Datasource, name, caption, full formula, then categorize by function:
   - Time comparison fields (date-relative booleans)
   - Measure swap / dynamic measure fields (CASE-driven)
   - LOD / aggregation fields (FIXED, INCLUDE, EXCLUDE)
   - Growth / delta / comparison fields
   - Filter / boolean fields
   - Domain-specific fields (lease, revenue, concession, ERG, etc.)
   - Utility / formatting fields
6. **Inventory worksheets** — Name, data source, visualization type, purpose; flag orphan sheets not on any dashboard
7. **Document dashboards** — Name, canvas size, layout zones, parameter controls, filter controls, embedded images, navigation elements
8. **Catalog actions** — Filter actions, highlight actions, URL actions, navigation actions, set actions
9. **Extract visual design** — Custom color palettes (name, type, hex values), custom shape palettes (name, shape count, description), typography, imagery/logos
10. **Document deployment** — Publishing config, extract refresh schedule, data refresh dependencies, maintenance considerations

### For .tds / .tdsx (Data Sources)

1. **Extract if packaged** — Unzip .tdsx to access .tds XML
2. **Parse connection details** — Server, port, database, schema, authentication method
3. **Catalog tables and joins** — Table names, join types, join clauses, relation structure
4. **Inventory columns** — Name, data type, role (dimension/measure), default aggregation, description
5. **Catalog calculated fields** — Same categorization approach as workbooks
6. **Document filters** — Data source filters, extract filters, context filters
7. **Extract metadata** — Groups, sets, bins, hierarchies, default properties
8. **Document refresh config** — Extract vs. live, refresh schedule, incremental refresh settings

### For .tfl / .tflx (Prep Flows)

1. **Extract if packaged** — Unzip .tflx to access .tfl JSON
2. **Map the flow structure** — Input steps, transform steps, output steps, flow direction
3. **Document input connections** — Source type, server, database, table/file, credentials method
4. **Catalog transformations** — Step name, step type, operation details:
   - Clean steps (rename, type change, filter, split, merge, calculated fields)
   - Aggregate steps (group-by fields, aggregation functions)
   - Pivot steps (pivot type, pivot fields, value fields)
   - Join steps (join type, join clauses, join results)
   - Union steps (union method, matched/unmatched fields)
   - Script steps (language, script content)
5. **Document calculated fields** — Name, formula, purpose (within each step)
6. **Document output configuration** — Output type, destination, file format, write mode (overwrite/append), published data source settings
7. **Map data lineage** — Field-level lineage from input through transformations to output

---

## Document Structure

Use the following section structure. Include only sections relevant to the file type being documented. Each section heading maps to the analysis steps above.

### Cover Page

```
[CLIENT NAME]
[Workbook / Data Source / Prep Flow Name]
Technical Documentation

| Field          | Value                              |
|----------------|------------------------------------|
| Client         | [Client name]                      |
| Asset Name     | [Name of workbook/datasource/flow] |
| Asset Type     | [Workbook / Data Source / Prep Flow]|
| Platform       | [Tableau Cloud/Server (region)]    |
| Version        | [Tableau version]                  |
| Revision       | [Current revision]                 |
| Document Date  | [Today's date]                     |
| Prepared By    | [Prepared by name]                 |
| Published URL  | [Link to live asset]               |
```

### 1. Executive Summary

A 2-3 paragraph overview covering:
- What the asset does and who it serves
- Data sources it connects to and upstream warehouse/system
- Key capabilities and metrics it delivers
- Refresh schedule and data currency
- Summary statistics table (dashboards, worksheets, data sources, parameters, calculated fields, actions, refresh cycle)

### 2. Architecture Overview

**For Workbooks:**
- Platform & hosting details (site, region, workbook ID, revision history, build version)
- Custom color palettes with hex values
- Dashboard layout summary table (name, dimensions, purpose, parameter control count)

**For Data Sources:**
- Connection architecture (server, port, authentication, database, schema)
- Table/relation structure

**For Prep Flows:**
- Flow topology diagram (inputs → transforms → outputs)
- Step count and type breakdown

### 3. Data Architecture

- Data source names, connection class, server, port, internal identifiers
- Table/relation structure for each data source
- Column families grouped by functional area (dimensions, measures, time, quality flags)
- For Prep flows: input source details and schema

### 4. Data Refresh & ETL Schedule

- Upstream warehouse refresh cycle (times, frequency, timezone)
- Tableau extract refresh alignment
- Data freshness monitoring fields or mechanisms
- Dependencies and failure modes
- _Include any client-provided refresh notes verbatim_

### 5. Parameters

_(Workbooks only)_
- Global control parameters table (name, data type, default, domain, purpose)
- Grouped parameter sets (e.g., floor plan inputs, scenario modeling pairs)
- Parameter-to-dashboard binding notes

### 6. Calculated Fields

- Total count and functional category breakdown table
- Detailed tables per category with field name, formula, and purpose:
  - Time Comparison
  - Measure Swap / Dynamic Measure
  - LOD / Aggregation
  - Growth / Delta / Comparison
  - Filter / Boolean
  - Domain-Specific (name the domain)
  - Utility / Formatting
- For Prep flows: calculated fields documented per step

### 7. Worksheets

_(Workbooks only)_
- Worksheets grouped by parent dashboard, in tables (name, data source, viz type, purpose)
- Orphan / viz-in-tooltip / hidden worksheets called out separately

### 8. Dashboards

_(Workbooks only)_
- Per dashboard: canvas size, layout description, component list, interactive controls table
- Parameter input systems and zone visibility logic

### 9. Actions & Navigation

_(Workbooks only)_
- Filter actions table (name, trigger, source, target, filter field, clearing behavior)
- URL actions, highlight actions, set actions
- Navigation patterns (image-based nav, button nav, menu sheets)

### 10. Transformations

_(Prep Flows only)_
- Step-by-step documentation of each transformation
- Join/union logic with field mappings
- Aggregation specifications
- Script step contents

### 11. Visual Design & Branding

_(Workbooks only)_
- Typography (primary/secondary typefaces)
- Color system table (color name, hex, usage)
- Custom shape palettes (name, count, description)
- Imagery and logo assets

### 12. Deployment & Maintenance

- Publishing configuration table (site, project path, name, revision, data source mode, refresh schedule)
- Data refresh dependency chain (step-by-step)
- Maintenance considerations and scaling limits
- Known issues or caveats

### 13. Appendix

- Full parameter reference table (all parameters with type, default, domain)
- Calculated field count by category
- Dashboard zone summary (sheets, param controls, filter controls, images, navigation per dashboard)
- For Prep: full field lineage mapping
- Data dictionary (if applicable)

---

## Formatting Standards

Regardless of output format, apply these standards:

- **Tables**: Use for all structured data (parameters, fields, worksheets, connections). Include header row with bold/shaded headers.
- **Formulas**: Display Tableau calc syntax in monospace/code formatting. Preserve exact field references including `[Parameters].[Name]` notation.
- **Hex colors**: Always include the `#` prefix. When documenting palettes, list all values.
- **Naming**: Use exact names from the workbook XML — do not rename or abbreviate field names, parameter names, or worksheet names.
- **Sections**: Number all top-level sections. Use sub-numbering (2.1, 2.2) for major subsections.
- **Branding**: Use the client's primary brand color for headers when producing styled output (Word/Google Doc/PDF). Default to dark navy (#0e2b4c) if no brand colors are identified in the workbook.

---

## Notes for Multi-File Documentation

When documenting multiple files in a single engagement:

- Produce one document per file unless the user requests a combined document
- Cross-reference shared data sources across workbooks (e.g., "This workbook connects to the same Curations data source documented in [Other Workbook] Technical Documentation")
- Note when a Prep flow is the upstream ETL for a workbook's data source
- Maintain consistent section numbering and formatting across all documents in the set
