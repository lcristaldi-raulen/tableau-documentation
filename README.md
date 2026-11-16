# Tableau Technical Documentation

A reusable framework for generating comprehensive technical documentation for Tableau workbooks, data sources, and Prep flows — across any client engagement.

## What's Included

| File | Purpose |
|------|---------|
| `template/Tableau Technical Documentation Template.md` | The full documentation template with section structure, analysis pipelines, and formatting standards |
| `skill/SKILL.md` | A Claude skill that automates the entire documentation process when you upload a Tableau file |

## Supported File Types

- **`.twbx` / `.twb`** — Tableau Workbooks (packaged or XML-only)
- **`.tds` / `.tdsx`** — Tableau Published Data Sources
- **`.tfl` / `.tflx`** — Tableau Prep Flows

## How It Works

### Using the Template (Manual)

1. Open a new Claude conversation
2. Paste the contents of `template/Tableau Technical Documentation Template.md` into the chat
3. Upload your Tableau file
4. Claude will prompt you for:
   - **Client name** and **prepared by** attribution
   - **Published URL** for the live asset
   - **Refresh schedule / ETL notes**
   - **Output format** (Word, Google Doc, Markdown, PDF)
5. Claude extracts, analyzes, and generates the full documentation

### Using the Skill (Automatic)

If you have Claude with Cowork mode:

1. Install the skill from `skill/SKILL.md` (save as `tableau-doc`)
2. In any conversation, upload a `.twbx`, `.tds`, or `.tfl` file and say "document this"
3. The skill triggers automatically — prompts for client info, analyzes the file, and delivers the documentation

The skill can also be invoked directly with `/tableau-doc`.

## Document Sections

Every generated document follows this structure (sections included only when relevant to the file type):

1. **Executive Summary** — What the asset does, key metrics, summary stats
2. **Architecture Overview** — Platform, hosting, color palettes, layout summary
3. **Data Architecture** — Connections, tables, column families
4. **Data Refresh & ETL Schedule** — Upstream warehouse cycles, extract refresh alignment
5. **Parameters** — Global controls, grouped parameter sets *(workbooks only)*
6. **Calculated Fields** — Categorized by function with formulas and purpose
7. **Worksheets** — Grouped by dashboard, with orphan sheet identification *(workbooks only)*
8. **Dashboards** — Layout, components, interactive controls *(workbooks only)*
9. **Actions & Navigation** — Filter/URL/highlight actions, nav patterns *(workbooks only)*
10. **Transformations** — Step-by-step transform documentation *(Prep flows only)*
11. **Visual Design & Branding** — Typography, colors, shapes, imagery *(workbooks only)*
12. **Deployment & Maintenance** — Publishing config, dependencies, scaling limits
13. **Appendix** — Full parameter reference, field counts, zone summaries

## Output Formats

- **Word (.docx)** — Styled with client brand colors, headers/footers, TOC
- **Google Doc** — Created directly in your Google Drive via HTML conversion
- **Markdown (.md)** — Clean markdown with tables and code-formatted formulas
- **PDF** — Rendered from Word with full styling

## Example

The Property Dashboard documentation for GMH Communities was generated using this framework:
- 36 worksheets, 3 dashboards, 90 parameters, 236 calculated fields
- 19-page Word document with branded headers and comprehensive appendix
- Delivered as Word, Google Doc, and Markdown

## Multi-File Documentation

When documenting multiple files in one engagement:
- One document per file (unless combined is requested)
- Cross-references shared data sources across workbooks
- Notes when a Prep flow is upstream ETL for a workbook's data source
- Consistent formatting across all documents in the set

---

**Created by** [XeoMatrix](https://xeomatrix.com)
