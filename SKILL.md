# Tableau Technical Documentation Skill

> **Skill name:** `tableau-doc`
>
> **Description:** Generate comprehensive technical documentation for Tableau workbooks (.twbx/.twb), data sources (.tds/.tdsx), and Prep flows (.tfl/.tflx). Triggers when the user uploads a Tableau file and asks to document it, or says "document this workbook", "create technical docs for this Tableau file", "tableau documentation", or similar. Also triggers on /tableau-doc.

---

You are generating comprehensive technical documentation for a Tableau asset. Follow every step below in order.

---

## Step 1 — Required Intake (Do Not Skip)

Before touching any file, collect this information from the user using the AskUserQuestion tool. If the user already provided some of these details in their message, only ask for what is missing.

### 1a. Client Information

Ask:
- **Client name** — the organization this asset belongs to (e.g., "GMH Communities", "Acme Corp")
- **Prepared by** — the team or company authoring the documentation (e.g., "XeoMatrix", "Bulletproof Analytics")

### 1b. Files and Links

For each file being documented, ask:
- **Published URL** — the Tableau Cloud/Server link to the live view (if applicable)
- **Known refresh schedule or ETL notes** — e.g., "refreshes every 6 hours from 8 AM to 11 PM aligned with BigQuery"
- If no file has been uploaded yet, prompt the user to upload or attach each `.twbx`, `.twb`, `.tds`, `.tdsx`, `.tfl`, or `.tflx` file

### 1c. Output Format

Ask how they want the documentation delivered:
- Word document (.docx)
- Google Doc
- Markdown (.md)
- PDF
- Multiple formats

---

## Step 2 — Extraction and Analysis

Once intake is complete, run the appropriate pipeline for each file type.

### For .twbx / .twb (Workbooks)

1. **Extract** — Unzip the .twbx into a working directory to access the .twb XML
2. **Parse with a script** — Write and run a Python script (use `python3 -I`) that parses the .twb XML and outputs a JSON analysis file containing:
   - `workbook_meta`: version, revision, platform, locale, site, server, build info
   - `data_sources`: for each — name, connection class, server, port, internal ID, tables/relations, column families
   - `parameters`: total count and a detail dict of every parameter with name, data type, default, domain type, domain values, caption
   - `calculated_fields`: total count and a list of every calculated field with datasource, column name, caption, full formula text
   - `worksheets`: total count, list of sheets with name, data source, and a list of orphan worksheets not placed on any dashboard
   - `dashboards`: for each — name, width, height, parameter controls, filter controls, worksheet zones, image zones, navigation elements
   - `actions`: filter actions, highlight actions, URL actions, set actions (name, trigger, source, target, fields)
   - `formatting`: custom color palettes (name, type, hex values), custom shape palettes (name, shapes), typography, embedded images
3. **Categorize calculated fields** — Pattern-match formulas into functional categories:
   - Time Comparison (date-relative booleans using DATEADD, date = parameter comparisons)
   - Measure Swap / Dynamic Measure (CASE [Parameter] WHEN patterns)
   - LOD / Aggregation ({ FIXED, { INCLUDE, { EXCLUDE expressions)
   - Growth / Delta / Comparison (arithmetic between current and prior period LODs)
   - Filter / Boolean (CONTAINS, IF/THEN returning true/false, parameter matching)
   - Domain-Specific (name the domain — lease, revenue, concession, ERG, occupancy, etc.)
   - Utility / Formatting (number formats, label generators, remaining/remainder calcs)

### For .tds / .tdsx (Data Sources)

1. **Extract if packaged** — Unzip .tdsx to get .tds XML
2. **Parse** — Script extracts: connection details (server, port, database, schema, auth), tables and joins (names, types, clauses), columns (name, type, role, aggregation), calculated fields, filters (data source, extract, context), metadata (groups, sets, bins, hierarchies), refresh config

### For .tfl / .tflx (Prep Flows)

1. **Extract if packaged** — Unzip .tflx to get .tfl JSON
2. **Parse** — Script maps: flow structure (inputs → transforms → outputs), input connections, every transformation step (clean, aggregate, pivot, join, union, script) with operation details, calculated fields per step, output config (destination, format, write mode), field-level lineage

---

## Step 3 — Generate the Document

Use the analysis JSON to write the full document following this exact section structure. Include only sections relevant to the file type. Use the appropriate output skill (docx, Google Workspace, pdf, or write markdown directly) based on the user's format choice.

### Cover Page

```
[CLIENT NAME]
[Asset Name]
Technical Documentation

| Field          | Value                              |
|----------------|------------------------------------|
| Client         | [Client name from intake]          |
| Asset Name     | [Name from XML/JSON]               |
| Asset Type     | [Workbook / Data Source / Prep Flow]|
| Platform       | [Tableau Cloud/Server (region)]    |
| Version        | [Tableau version from meta]        |
| Revision       | [Revision from meta]               |
| Document Date  | [Today's date]                     |
| Prepared By    | [From intake]                      |
| Published URL  | [From intake, or N/A]              |
```

### 1. Executive Summary
2-3 paragraphs: what the asset does, who it serves, data sources and upstream warehouse, key capabilities, refresh schedule. End with a summary statistics table (dashboards, worksheets, data sources, parameters, calculated fields, actions, refresh cycle).

### 2. Architecture Overview
**Workbooks:** Platform/hosting details, custom color palettes with hex values, dashboard layout summary table.
**Data Sources:** Connection architecture, table/relation structure.
**Prep Flows:** Flow topology, step count and type breakdown.

### 3. Data Architecture
Data source names, connection class, server, port, internal IDs. Table/relation structure. Column families grouped by functional area. For Prep: input source details and schema.

### 4. Data Refresh & ETL Schedule
Upstream warehouse refresh cycle (times, frequency, timezone). Tableau extract refresh alignment. Data freshness monitoring. Dependencies and failure modes. Include user-provided refresh notes verbatim.

### 5. Parameters _(Workbooks only)_
Global control parameters table (name, data type, default, domain, purpose). Grouped parameter sets. Parameter-to-dashboard binding notes.

### 6. Calculated Fields
Total count and category breakdown table. Detailed tables per category with field name, formula, and purpose. For Prep: calculated fields per step.

### 7. Worksheets _(Workbooks only)_
Grouped by parent dashboard in tables (name, data source, viz type, purpose). Orphan/viz-in-tooltip/hidden sheets listed separately.

### 8. Dashboards _(Workbooks only)_
Per dashboard: canvas size, layout description, component list, interactive controls table. Parameter input systems and zone visibility logic.

### 9. Actions & Navigation _(Workbooks only)_
Filter actions table. URL, highlight, set actions. Navigation patterns (image nav, button nav, menu sheets).

### 10. Transformations _(Prep Flows only)_
Step-by-step documentation. Join/union logic with field mappings. Aggregation specs. Script step contents.

### 11. Visual Design & Branding _(Workbooks only)_
Typography. Color system table (name, hex, usage). Custom shape palettes. Imagery and logo assets.

### 12. Deployment & Maintenance
Publishing config table. Data refresh dependency chain. Maintenance considerations and scaling limits. Known issues.

### 13. Appendix
Full parameter reference table. Calculated field count by category. Dashboard zone summary. For Prep: full field lineage mapping. Data dictionary if applicable.

---

## Formatting Standards

Apply these regardless of output format:

- **Tables** for all structured data. Header rows with bold/shaded headers.
- **Formulas** in monospace/code formatting. Preserve exact `[Parameters].[Name]` notation.
- **Hex colors** always include the `#` prefix.
- **Naming** — use exact names from the workbook XML. Never rename or abbreviate.
- **Sections** — number all top-level sections. Sub-number (2.1, 2.2) for major subsections.
- **Branding** — use the client's primary brand color for headers in styled output. Default to dark navy `#0e2b4c` if no brand colors found.

---

## Multi-File Documentation

When documenting multiple files in one session:

- One document per file unless user requests combined
- Cross-reference shared data sources across workbooks
- Note when a Prep flow is upstream ETL for a workbook's data source
- Consistent numbering and formatting across all documents

---

## Output Delivery

After generating the document:

1. **Verify** — For Word/PDF, convert to PDF and visually inspect rendered pages. For Google Doc, read back content to confirm.
2. **Share** — Use present_files for Word/PDF/MD, or provide the Google Doc link.
3. **Offer additional formats** — Ask if the user wants the same content in another format.
