# Assumption review before implementation

Codex wrote the initial draft with the user's accepted scope, area, and submission definition. A separate review agent read UC-WAR-nudge-non-submitters and its cited rules before any feature code was written. This is an agent-assisted draft, not a claim of unaided student authorship.

Initial specification commit: `0b4cf02`.
Build context: [issue #1](https://github.com/Tee123455/tee-spec-a-feature/issues/1). Its body contains only the use case citation and expected changed paths.

The reviewer supplied these assumptions. Each requirement gap was routed to its source of truth:

| Review assumption | Home and resolution |
| --- | --- |
| “The current week, week boundaries, configured due day, and configured due time all use the same explicitly chosen application timezone.” | Business rule: BR-war-nudge-period now uses ISO weeks and app.timezone, matching the existing reminder clock. |
| “Deactivated students remain eligible while enrolled and assigned to a team.” | Business rule: rejected; BR-war-nudge-eligibility requires an active account, and the use case cites BR-student-lifecycle. |
| “A student in cooldown still appears in the non-submitter list, with the next permitted attempt time, and cannot be selected until that time.” | Use case: adopted in the display step and empty/disabled-send extension. This is better than hiding the student: it explains why a missing report cannot currently be nudged. |
| “The authoring link in the email uses the configured public frontend URL and opens the recipient’s weekly activity authoring page.” | Use case: Associated Information specifies this destination and authentication. |
| “The preview is regenerated from a server-controlled template; instructors cannot edit the message.” | Use case: fixed message control now specified. |
| “An interrupted batch retains completed outcomes; recipients not yet attempted consume no allowance.” | Use case: interruption extension preserves outcomes and reports unattempted recipients. |
| “If SMTP acceptance cannot be determined after a timeout or process interruption, the attempt consumes the cooldown and is reported as failed with an uncertainty explanation.” | Business rule sets allowance consumption; use case specifies the visible outcome. |
| “An attempted delivery is durably recorded before SMTP is contacted, and concurrent requests reserve the shared cooldown atomically.” | Business rule: BR-war-nudge-cooldown now requires atomic durable reservation. |
| “A section-access or period failure arising midway through a batch returns the outcomes already completed and a clear reason why remaining recipients were not attempted.” | Use case: adopted for period failure; after access loss, student details remain withheld until authorization is restored. |
| “Audit records survive student deletion until institutional disposal policy permits deletion.” | Use case: independent identifier-only outcome records survive deletion, without copied names or addresses. Existing lifecycle and retention rules remain authoritative. |
| “Audit retention follows an institution-supplied policy without introducing an invented expiration period.” | Use case: cites existing retention policy without inventing a duration. |
| “Delivery outcomes are accessible through the completed-send response; a separate historical audit browser is outside this feature.” | Use case: adds retrieval of a batch result after interruption; a general audit browser remains outside scope. |
| “Malformed, empty, or duplicate selection identifiers are validated before any email is attempted; duplicates produce only one recipient attempt.” | Use case: request-level validation extension, with duplicates normalized. |

Build-context gap: the initial context named activity.js and router/index.js; inspection showed TypeScript activity/index.ts, activity/types.ts, and router/routes.ts. The filed issue uses the real paths. File locations belong in the issue, not in behavioral requirements.

Deliberately derived choices: endpoint and DTO names, Vue layout, sorting, persistence representation, status enums, and exact template wording within required content and privacy constraints. Different choices still satisfy the same observable requirements. Atomic enforcement is required, but the locking or transaction mechanism is not prescribed.

No feature code was implemented. The follow-up commit records the actual review fixes separately from the initial specification.
