---
name: siwes
description: Generate a complete, formatted Nigerian university or polytechnic SIWES technical report as a downloadable .docx file with clean image placeholders. Trigger when the user types /siwes or asks to write, draft, transcribe, or format a 12-week or 6-month (24-week) SIWES industrial training report from weekly logbook notes, scanned PDFs, handwritten logbook photos, and formatting guidelines.
---

# SIWES Technical Report Generator (`/siwes`)

Generate a comprehensive, professionally formatted SIWES (Student Industrial Work Experience Scheme) Technical Report as a downloadable `.docx` file tailored to Nigerian university and polytechnic standards.

## Installation & Setup (For Users)

If someone asks how to install or share this skill:

- **Via `npx skills`:** `npx skills add judexify/siwes-skill`
- **Via Claude Code Plugin Marketplace:** `/plugin marketplace add judexify/siwes-skill`
- **Required Python Package:** `pip install python-docx`

## Purpose & Design Philosophy

- **Fixed Format, Flexible Input:** The structural and formatting standard stays consistent across every run, while the weekly logbook content changes for each student (supporting both 12-week and 6-month / 24-week placements).
- **Multi-Format Logbook Support:** Accepts typed text/Word files, scanned PDFs of handwritten logbooks, or batches of clear photos of logbook pages—transcribing handwritten entries first before synthesizing the report.
- **Text-Only Content Generation with Image Placeholders:** Generates the complete written report and inserts clean, clearly labeled `[INSERT FIGURE X.X HERE: ...]` boxes wherever diagrams, organograms, or site snapshots belong, so the student can manually drop their own images into the `.docx` after generation. Do NOT auto-generate or embed images unless explicitly asked in that specific turn.

## When to Use

- The user triggers `/siwes` or asks to generate, write, or compile a SIWES technical report for themselves or a classmate.
- The user provides a student's 12-week or 6-month (24-week) SIWES logbook records (as pasted text, `.txt`/`.docx`, scanned `.pdf`, or handwritten page photos) along with placement details.

## Reference Assets

This skill includes a permanent formatting and structural reference in `references/`:

- `references/sample-siwes-report.docx`: Structural and stylistic reference model showing standard preliminary pages, four-chapter breakdown, tone, tables, image placeholder conventions, and Nigerian university/polytechnic formatting.

Before generating the `.docx`, inspect `references/sample-siwes-report.docx` if needed to ensure alignment with the established layout and tone.

## Workflow

### Step 1: Gather & Verify Per-Student Inputs

For each report run, check that the user has supplied:

1. **Student & Placement Metadata:** Full name, matriculation/registration number, institution, department, degree/award (`B.Eng`, `B.Tech`, `HND`, `ND`), host organization name and address, department/unit attached to, duration (e.g., 12 weeks or 24 weeks / 6 months), industry supervisor name, and completion month/year.
2. **That Student's Logbook Source (Any Format):**
   - Typed/pasted weekly notes (`.txt`, `.md`, `.docx`, or inline chat text), **OR**
   - Scanned PDF of a handwritten logbook (e.g., `./my-siwes-logbook.pdf`), **OR**
   - Photos of handwritten logbook pages (`.jpg`, `.png` — uploaded directly or in a local folder).
3. **Optional Custom Guideline:** Any department-specific tweaks for that student. Otherwise, always apply the built-in formatting standard:
   - Font: **Times New Roman**, 12pt body text, 1.5 line spacing, justified alignment.
   - Margins: 1.5 inches left (for binding), 1.0 inch top, bottom, and right.
   - Page numbering: Roman numerals (`i, ii, iii...`) centered at the bottom for preliminary pages; Arabic numerals (`1, 2, 3...`) starting on Chapter One Page 1.

If the student's details or logbook source are missing, ask a brief, direct question to collect them before proceeding.

### Step 2: Transcribe Handwritten / Scanned Logbooks (If Applicable)

If the user supplies photos or a scanned PDF of a handwritten logbook (especially for 6-month / 24-week placements):

1. **Batch Transcription:** Read the pages in chronological order (processing 4–6 weeks per pass for long 24-week logbooks so no technical details are skipped) and save a clean markdown copy (`transcribed-logbook.md`) in the working directory.
2. **Verify Unclear Handwriting:** If any technical term, component rating, or supervisor name is illegible, flag it briefly for the user to confirm while continuing with the rest of the transcription.
3. **Proceed to Synthesis:** Once all weeks are transcribed, use the full verified logbook to build the report.

### Step 3: Structure the Report

Follow this fixed report structure across every generated document:

1. **Preliminary Pages** (each starting on a new page):
   - **Title Page:** Uppercase centered formal title block, host organization, student name & matric number, department & award statement, month & year.
   - **Certification:** Declaration statement with signature/date lines for Student, Industry Supervisor, and Institutional SIWES Supervisor.
   - **Dedication**
   - **Acknowledgement**
   - **Abstract:** Concise single-page summary covering placement location, duration, core practical activities, tools used, skills acquired, and overall impact.
   - **Table of Contents**, **List of Figures**, and **List of Tables** (dynamically matching the generated sections, figure placeholders, and tables).

2. **Chapter One — Introduction**
   - `1.0 Introduction`
   - `1.1 Background to the Study` (History of ITF establishing SIWES in 1973 and Federal Government approval in 1974)
   - `1.2 Overview of the SIWES Programme`
   - `1.3 Aim and Objectives of SIWES`
   - `1.4 Significance of SIWES`
   - `1.5 Chapter Summary`

3. **Chapter Two — Description of the Host Organisation**
   - `2.0 Introduction`
   - `2.1 Company / Organisation Profile and Introduction`
   - `2.2 Organisational Set-Up / Organogram` (Include a bordered image placeholder box for the company organogram)
   - `2.3 History and Background`
   - `2.4 Vision, Mission, and Core Values`
   - `2.5 Professional Ethics and Code of Conduct`
   - `2.6 Chapter Summary`

4. **Chapter Three — Industrial Experience / Work Done**
   - `3.0 Introduction to Industrial Experience`
   - `3.1 Departments and Units Worked In` (Include an image placeholder box for the department/workplace snapshot)
   - `3.2 Activities and Tasks Performed` — Synthesize that specific student's weekly logbook entries into rich, cohesive technical prose grouped logically by functional themes, departments, or progressive phases (never raw daily bullet dumps), preserving exact technical specifications (ratings, sizes, models, procedures, and site operations). Insert clean image placeholder boxes at logical points for schematic diagrams and site work snapshots.
   - `3.3 Skills and Competencies Gained` (Technical skills and Soft skills)
   - `3.4 Tools, Instruments, and Equipment Used` (Structured Word table categorizing Hand tools, Testing/Measuring instruments, Materials/Components, and PPE, followed by an image placeholder box for tools/equipment)
   - `3.5 Challenges Encountered and Solutions`
   - `3.6 Chapter Summary`

5. **Chapter Four — Conclusion, Limitations and Recommendations**
   - `4.0 Conclusion`
   - `4.1 Limitations of the Training`
   - `4.2 Recommendations` (For students, host organizations, and institutions)
   - `4.3 Chapter Summary`

6. **References & Appendices**
   - Formal references list (ITF guidelines, departmental handbook, technical manuals).
   - Appendices (Weekly logbook summary table covering all 12 or 24 weeks, plus placeholders for supplementary diagrams/snapshots).

### Step 4: Insert Clean Image Placeholders

- Everywhere a schematic diagram, organogram, or site snapshot belongs, insert a clean, bordered single-cell box (or shaded frame) in the `.docx` with centered bold instructions (e.g., `[INSERT FIGURE 3.1 HERE: Schematic Diagram / Snapshot of Distribution Board Termination]`) followed by a centered caption line directly underneath (e.g., **Figure 3.1: Distribution Board Termination Layout**).
- List every figure placeholder accurately in the preliminary **List of Figures** so the user only needs to drop their images into the boxes after downloading.

### Step 5: Generate & Deliver the `.docx` File

- Use `python-docx` to build the complete `.docx` file with proper XML section breaks (so Roman numerals on preliminary pages restart at Arabic `1` on Chapter One), styled headings, clean bordered tables, and image placeholder boxes.
- Write thorough, formal academic prose—expand terse weekly logbook entries into clear technical explanations.
- Save the file using the student's name (e.g., `SIWES-Report-<Student-Name>.docx`) in the working directory and notify the user.

## Usage Examples

### Example 1: Typed / Pasted Logbook Notes

```text
/siwes

Student Name: Adebayo Oluwaseun
Matric Number: EEE/21/0142
Institution: Federal University of Technology, Akure (FUTA)
Department: Electrical and Electronics Engineering
Award: B.Eng in Electrical and Electronics Engineering
Host Organisation: Kwara State Ministry of Energy, Ilorin
Department/Unit Attached: Engineering Services (Transmission & Distribution)
Duration: 12 Weeks (August – November 2026)
Industry Supervisor: Engr. S. A. Balogun

Weekly Logbook Notes:
[Paste or attach weekly logbook entries]
```
