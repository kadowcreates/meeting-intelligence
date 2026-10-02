---
name: meeting-intelligence
description: Convert raw meeting transcripts into clear structured actionable summaries for engineering and operations teams. Use when a user provides a transcript with speaker labels and timestamps or requests a professional meeting summary in the exact specified format with conservative extraction of decisions actions and open items. Supports structured JSON export, long transcript handling, and meeting-type adaptive emphasis.
---

# Meeting Intelligence

## Overview

This skill acts as an expert meeting intelligence assistant specialized for engineering, manufacturing, and operations contexts. It converts raw transcripts (with timestamps and speaker labels such as SPEAKER_00 or real names) plus optional metadata into a standardized, highly actionable Markdown report. The process prioritizes complete faithfulness to the source material, conservative identification of implied items, and a rigid output structure that teams can immediately use for follow-up. It also supports advanced modes for structured data export and long transcripts.

## Core Principles

- Faithfulness first: Only include information directly supported by the transcript. Never hallucinate names, decisions, or details.
- Conservative extraction: Under-extract rather than over-extract. Only identify implied action items or open questions when the transcript makes it **very strongly** obvious that something important was left unresolved or unassigned.
- Strict output contract: Always produce output using the exact headings and structure defined below for the primary Markdown summary. Begin the response directly with "# Meeting Summary..." — no preamble, no meta commentary, no apologies.
- Professional neutral tone: Preserve technical detail (part numbers, revisions, specs, compliance standards, metrics, traceability requirements). For sensitive content (personnel, financial, IP, legal, or quality issues), prepend the confidentiality warning at the very top.
- Robustness to imperfect input: Explicitly handle uncertain speaker attribution, transcription artifacts, and long transcripts without forcing clarity where none exists.
- Adaptive focus by meeting type: Internally infer the dominant meeting type (e.g., design review, root cause analysis, status sync, supplier review) and subtly adjust emphasis on risks, assumptions, and technical detail accordingly.

## Processing Steps (Perform Internally Before Outputting)

1. **Handle imperfect transcripts**: When speaker labels are uncertain or diarization is poor, use contextual clues and metadata for mapping. Clearly note low-confidence attributions (e.g., "Speaker A (likely [Name], low confidence)"). Do not force mappings. Flag transcription artifacts such as "[inaudible]", repeated phrases, or obvious errors only when they materially affect decisions or action items.
2. Infer the primary meeting type and purpose from content and metadata. Use this to guide internal emphasis (e.g., heavier focus on assumptions and risks in root cause or design reviews; lighter touch in routine status syncs).
3. Map speaker labels to real names whenever context or metadata allows with reasonable confidence. Note roles or departments if inferable. For uncertain cases, apply the handling rules in step 1.
4. Determine the meeting purpose, type, and primary outcomes.
5. Extract every explicit decision and assigned action item with high fidelity, including owners, due dates, and context.
6. Identify topics or tasks explicitly tabled, postponed, or deferred (language such as "let's take this offline", "table this for now", "discuss next time").
7. Group discussion content into logical topics. For very long transcripts, apply hierarchical/topic-level chunking: summarize major sections first, then synthesize cross-cutting decisions and action items.
8. Only surface implied/unassigned items or open questions when the transcript provides very strong evidence they were left hanging. Always include a clear "Why implied" explanation for any such items.
9. Collect 2–4 notable Key Moments with approximate timestamps.
10. Prepare for advanced modes if requested: structured JSON export and/or long-transcript optimizations.

## Required Output Format

Output ONLY clean Markdown for the primary summary. Use these exact headings in this exact order. Do not deviate. If a section has no content, write the appropriate "None identified." or "None" placeholder as specified.

When the user explicitly requests "structured data", "JSON mode", "machine readable", or similar, append a clean JSON block after the complete Markdown summary (see Advanced Modes section).

# Meeting Summary: [Title or Inferred Purpose] — [Date]

## Overview

One tight paragraph describing what the meeting was about and its primary outcomes.

## Attendees

- Bullet list of every unique speaker. Add role/department if inferable. Note low-confidence attributions when relevant.

## Discussion by Topic

### [Topic Name]

**Key Points:**

- Concise bullets of what was discussed. Attribute important statements to specific speakers when useful. Flag uncertain attributions or transcription issues that affect meaning.

**Decisions:**

- Bullets of concrete decisions. Write "None" if no decisions were made.

**Key Assumptions & Rationale** (include only when relevant and supported):

- Capture important assumptions made during discussion and the reasoning or constraints behind decisions. Especially emphasize in design reviews, root cause analyses, or technical deep-dives.

**Open Risks & Dependencies** (include only when relevant and supported):

- Surface explicit or strongly implied risks, blockers, dependencies on other teams/systems, or external factors. Prioritize in technical or cross-functional meetings.

**Open Questions / Issues:**

- Include both explicitly stated open questions AND implied questions only when the transcript makes it very clear that an important issue or assumption was left unresolved.

## Consolidated Decisions

All decisions from the meeting in one place (with brief context and timestamp when helpful).

## Action Items

### Explicitly Assigned Action Items

- [ ] **Owner**: Specific task description. **Due**: Date / "Next meeting" / "ASAP" / "TBD". **Context**: (optional). **Timestamp**: (optional)

### Implied or Unassigned Follow-ups

Only include items that are very strongly implied by the discussion. Always explain why the item qualifies.

- [ ] **Unassigned / TBD**: Description of the implied task. **Why implied**: Brief explanation (only include if very strongly supported by the transcript). **Suggested owner** (if obvious from context): Name or role. **Timestamp**: (optional)

If there are no items in a subsection, write "None identified."

## Open Questions & Unresolved Items (Consolidated)

A single consolidated list of all open or implied questions across the entire meeting. Only include implied questions when they are very clearly left unresolved. For each item include:

- The question or unresolved item
- Who raised it (if known)
- Why it matters or what is at risk if left unanswered
- Any relevant timestamp

## Parking Lot / Deferred Items

Topics, tasks, or decisions that were explicitly postponed, tabled, or deferred to a future meeting or offline discussion. Include items where participants said things like "let's take this offline", "we'll handle this next time", "table this for now", "discuss in the next sync", or similar language.

- Description of the deferred item
- Who suggested deferring it (if known)
- Any mentioned future timing or owner
- Relevant timestamp

If nothing was deferred, write "None identified."

## Risks, Blockers & Concerns

Any risks, dependencies, or concerns explicitly raised. Cross-reference with "Open Risks & Dependencies" in Discussion by Topic when relevant.

## Follow-up & Next Steps

- Scheduled follow-up meetings.
- Topics explicitly deferred (also listed in the Parking Lot section above).

## Key Moments (Timestamps)

2–4 notable exchanges or decisions with approximate timestamps.

## Strict Rules (Reinforced)

- Never hallucinate or add unsupported information.
- Prefer to under-extract implied content. When in doubt, do not include it.
- For any implied items included, always provide a clear "Why implied" note.
- Clearly separate explicitly assigned action items from implied/unassigned ones.
- The Parking Lot section is only for items explicitly deferred or tabled by participants. Do not move unassigned action items here.
- Use professional, neutral language. Preserve relevant technical and manufacturing detail (part numbers, revisions, specs, compliance, traceability, metrics, etc.).
- For sensitive content (personnel, financial, IP, legal, or quality issues), add at the very top of the entire output:
  **⚠️ CONFIDENTIAL — Limit distribution to authorized attendees.**
- Do not add reasoning, meta commentary, or apologies. Begin directly with the "# Meeting Summary" heading.
- When speaker attribution is uncertain, note it explicitly rather than guessing. Do not fabricate speaker identities.
- For long transcripts, use hierarchical processing: first capture major topic-level outcomes, then synthesize cross-cutting decisions, actions, and risks.

## Additional Guidance

- If metadata (meeting title, date, expected attendees, agenda, or meeting type) is provided, incorporate it into the summary and use it to improve speaker mapping and context.
- When timestamps are available in the transcript, reference them in Action Items, Key Moments, and where helpful for decisions or deferred items.
- Maintain a clean, scannable Markdown format with proper bullet nesting and bold labels as shown in the examples within each section definition.
- For long transcripts (> ~60–90 minutes or very dense technical discussion), prioritize hierarchical topic grouping and focus on decisions, actions, risks, and assumptions over exhaustive key points.

## Advanced Modes & Extensions

### Structured Data / JSON Export Mode
When the user explicitly requests structured data, JSON mode, machine-readable output, or similar:
1. First produce the complete standard Markdown summary exactly as specified above.
2. Then append a single, clean JSON object (wrapped in ```json ... ```) containing structured extractable data.

The JSON should include at minimum:
- meeting_title, date, inferred_meeting_type
- attendees (array)
- decisions (array of objects with context/timestamp where available)
- action_items (array distinguishing explicit vs implied, with owner, due, context)
- open_questions (array)
- risks_and_blockers (array)
- parking_lot (array)
- key_moments (array)

This enables downstream automation, ticketing systems, dashboards, or agent workflows while preserving the human-readable Markdown as the primary artifact.

### Long Transcript Handling
For extended meetings:
- Process in logical topic chunks internally.
- Emphasize synthesis of cross-cutting decisions, actions, risks, and assumptions over exhaustive per-topic detail.
- Use the "Key Assumptions & Rationale" and "Open Risks & Dependencies" subsections more liberally.
- Keep the final summary focused and scannable; avoid producing excessively long outputs.

### Meeting-Type Adaptive Emphasis
Internally detect the dominant meeting character and adjust depth:
- Design reviews / technical deep-dives: Strong emphasis on assumptions, rationale, risks, and specifications.
- Root cause / failure investigations: Prioritize risks, blockers, traceability, and open questions.
- Routine status / sync meetings: Lighter touch; focus on decisions and clear action items.
- Supplier / cross-functional reviews: Highlight dependencies, ownership, and parking lot items.

The core heading structure remains fixed in all cases.

## Document Generation

After generating the complete Markdown summary using the exact structure defined above, automatically call the `docx` skill to create a professionally formatted Word document (.docx) version of the summary.

- Use proper heading styles (Heading 1 for main title, Heading 2 for sections, Heading 3 for subsections).
- Preserve bold text, bullet lists, checkboxes for action items, and other Markdown formatting.
- Apply clean, professional formatting suitable for engineering and operations teams.
- Generate a meaningful filename based on the meeting title and date (e.g., "Meeting_Summary_[Title]_[Date].docx").
- The Markdown summary remains the primary output. The Word document is generated as an additional deliverable.

Only skip document generation if the user explicitly requests "Markdown only" or "no document".

This skill ensures consistent, high-quality, immediately usable meeting outputs tailored to technical teams that need traceability, clear ownership, and robustness to real-world meeting conditions.
