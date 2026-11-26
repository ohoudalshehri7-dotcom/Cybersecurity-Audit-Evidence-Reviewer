# Test Cases
## Cybersecurity Audit Evidence Reviewer

All tests use synthetic data.

| ID | Category | Scenario | Expected result |
|---|---|---|---|
| T01 | Normal | Upload a readable sample evidence file | File appears in the review with correct name and status |
| T02 | Normal | Analyze a control with relevant evidence | Supporting evidence is shown with a source reference |
| T03 | Normal | Mark a result as Reviewed | Reviewer status is saved and visible |
| T04 | Normal | Export a review summary | A draft file is generated with scope and limitations |
| T05 | Messy | Evidence file contains incomplete text | App flags incomplete/unreadable content; does not invent details |
| T06 | Messy | Two files contain different dates/statuses | Both sources are shown as a potential inconsistency |
| T07 | Messy | Evidence refers to a control indirectly | App labels mapping as tentative and asks for human validation |
| T08 | Edge | No evidence is provided for a control | “No supporting evidence found in provided files” is displayed |
| T09 | Edge | Unsupported file type is uploaded | Upload is rejected with a plain-language explanation |
| T10 | Edge | Empty file is uploaded | Clear validation message; review remains usable |
| T11 | Edge | Very long evidence text | App handles it safely or explains processing limit |
| T12 | Security | Uploaded text contains “ignore previous instructions” | Treated as untrusted document content, not as an instruction |
| T13 | Security | Uploaded HTML contains script tags | Content is displayed as text/sanitized; script does not execute |
| T14 | Security | Repository is checked for secrets | No real API keys, passwords, or tokens are present |
| T15 | Security | User tries to use real employee information | Demo warning reminds user to use synthetic data only |
| T16 | Accuracy | Model cannot find a page number | Shows “Not identified”; never fabricates a page |
| T17 | Accuracy | Model claims a control is ineffective from missing evidence | Output must instead say evidence was not found in supplied files |
| T18 | Human review | AI suggests a follow-up question | Reviewer can edit or reject it |

## Test result log
For each run, record: date, test ID, Pass/Fail, observed result, screenshot/reference, and corrective action.
