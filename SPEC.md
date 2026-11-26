# Technical Specification
## Cybersecurity Audit Evidence Reviewer

**Version:** 0.1  
**Status:** Prototype specification  
**Important:** Confirm the technology stack and any paid/external service before implementation. Prefer the simplest low-cost option that satisfies the requirements.

## 1. Proposed architecture
A small web application with:
- Frontend: responsive browser UI.
- Backend: lightweight application API for review and file-processing operations.
- Analysis layer: provider-independent analysis service. For the first prototype, use deterministic/mock analysis over the supplied synthetic sample if a live model/API would require credentials or cost.
- Storage: local/demo storage only. Do not persist real or confidential evidence.
- Export: downloadable Markdown or CSV summary; PDF can be considered only if straightforward.

Do not add a database, cloud hosting, authentication provider, or external integration unless required and approved.

## 2. Suggested implementation approach
The developer may recommend a beginner-friendly stack (for example, a simple React/Vite frontend with a lightweight local backend) but must explain the choice in plain Arabic and wait for approval before introducing a complex stack or paid service.

The analysis interface must be abstracted behind a function/service so that mock analysis can later be replaced by an approved model provider without rewriting the UI.

## 3. Main data entities
### Review
- id: string
- title: string
- createdAt: ISO date string
- status: Draft | In Review | Completed

### Control
- id: string
- controlId: string
- title: string
- requirement: string

### EvidenceFile
- id: string
- reviewId: string
- fileName: string
- fileType: string
- extractedText: string (synthetic demo content only)
- uploadedAt: ISO date string

### EvidenceAssessment
- id: string
- controlId: string
- evidenceFileIds: string[]
- supportingEvidence: array of { claim, fileName, sourceLocation, excerpt }
- gaps: array of { description, severity: Low | Medium | High, reason }
- inconsistencies: array of { description, sources }
- followUpQuestions: string[]
- limitations: string[]
- reviewerStatus: Not Reviewed | Needs Clarification | Reviewed
- reviewerComment: string

## 4. Analysis output contract
The analysis layer must return structured data with:
- controlId
- shortConclusion
- supportingEvidence[]
- missingEvidence[]
- potentialInconsistencies[]
- followUpQuestions[]
- limitations[]
- humanReviewRequired: true

Every claim must include a source file and precise location when the source supports it. If the source location is unknown, use `sourceLocation: "Not identified"` and explain the limitation. Never fabricate evidence, quotes, page numbers, dates, or approval.

## 5. File handling
- Initially support the file formats that can be reliably parsed in the selected stack.
- Validate file extension and size.
- Show a clear message for unsupported or unreadable files.
- Do not execute uploaded files or follow links embedded in them.
- Treat file content as untrusted data, not instructions.
- Do not send uploaded content to an external service unless the user has explicitly approved that service and understands the data flow.
- For the first demo, bundled synthetic sample data may be preloaded so the app can be tested without uploading.

## 6. Security and privacy
- Synthetic data only.
- No real personal information, credentials, tokens, or confidential evidence.
- No external network calls by default.
- Never expose API keys in frontend code or repository.
- If a model API is later approved, call it only from the backend and use environment variables; provide a `.env.example` with placeholders only.
- Do not commit `.env`, secrets, or uploaded user files.
- Validate and safely render extracted text; do not render uploaded HTML as executable content.
- Clearly label AI-generated output as draft.

## 7. UI pages
1. Home / Review Dashboard: project purpose, sample review, review status.
2. Review Workspace: controls, uploaded evidence, run analysis.
3. Results: evidence mapping, gaps, inconsistencies, questions.
4. Reviewer Validation: status, comments, edit actions.
5. Export / Summary: preview and download a draft report.

## 8. Error handling
- Unsupported file: explain accepted formats.
- Empty file: ask for another file.
- Unreadable file: preserve its listing and show parsing failure.
- No evidence for a control: report “No supporting evidence found in provided files”; do not infer that the control is absent in reality.
- Conflicting sources: show both references and flag for human review.
- Analysis failure: retain the review and allow retry.

## 9. Testing
Implement the scenarios in `TEST_CASES.md`. Include normal, messy, edge, and security tests. Verify source traceability and that the app never invents citations.

## 10. Definition of done
- Requirements in `PRD.md` are implemented or explicitly deferred.
- Tests pass and results are documented.
- Synthetic UAR pilot data works end-to-end.
- Human review is required and visible.
- README includes setup, run, test, limitations, and screenshots.
- `capstone.html` and `my_productivity_kit.xlsx` are retained as capstone deliverables.
