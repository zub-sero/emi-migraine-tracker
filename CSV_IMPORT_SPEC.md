Emi: CSV Import — Design Specification
The problem
Most people who track migraines seriously already have a system: a personal spreadsheet, a paper diary, or a clinic-provided template. Every format I looked at, including official templates from the National Headache Foundation and the National Migraine Centre, uses a similar shape but with no standardisation between them: different column names, different time formats, different severity scales, free-text medication entries.
If Emi cannot import that history, every new user faces a choice between abandoning months or years of data, or manually re-entering it one row at a time. That is a real adoption barrier, not a minor inconvenience. The aim of this feature is to remove this barrier.

Why start off with rules, not AI
Most of the mapping problem is deterministic. Column headers like "Pain score" or "Severity (1-10)" can be matched to a known field with simple fuzzy string matching, reliably, for free. I chose to reserve AI for only the cases a rule-based system cannot resolve confidently, as this is both cheaper to run and easier to resolve when something goes breaks, since a human can read the rule that failed rather than guess why a model made a particular call.
This also means the feature is useful in two stages rather than one. Phase 1 (rule-based mapping) works as a complete, shippable feature on its own. Phase 2 (AI escalation for low-confidence cases) is an enhancement layered on top later.

Design principle
Where the importer cannot determine something with confidence, it says so rather than guessing. A visible gap in a user's imported history is more trustworthy than a plausible-looking number that may be wrong. Every approximation the importer makes (for example, converting "moderate" to a numeric severity, or "all day" to a duration) is flagged, never presented as equivalent to data the user explicitly recorded.

What real tracker data revealed
Designing against synthetic test data would have missed real-world patterns. Testing against actual migraine diaries surfaced several gaps in Emi's original schema:
* Severity is commonly recorded with half-point precision (7.5, not just whole numbers)
* Time is often recorded as a vague period ("morning", "all day") rather than an exact time
* Medication is frequently a combination of drugs in one cell ("Domperidone + 1g Panadol"), often using brand names rather than generic drug names
* Daily preventative medication is a standing fact about the user, not an event tied to a specific migraine, and needs its own data model entirely separate from per-episode logging
* Some rows in a real tracker are not migraine entries at all, but notes about starting or stopping a medication

Each of these became a deliberate schema change, made before building the importer rather than worked around afterwards.

Phase 1: rule-based mapping
Column mapping. Source column headers are matched to Emi's fields using fuzzy string matching against a synonym list (for example, "Pain score", "Severity (1-10)", and "Intensity" all map to severity). Exact matches are mapped automatically with high confidence; partial matches are mapped but flagged for review; anything below a reasonable match threshold is left for the user to assign manually.
Row classification. Every row is classified before any field parsing happens: a genuine migraine entry, a medication status note (such as "started new preventative"), or junk/unusable. This matters because not every row in a real tracker represents an episode, and treating a medication note as a malformed entry would lose real information rather than just failing to import it.
Field parsing. Within genuine entries, severity, duration, and medication fields are parsed against the inconsistent formats found in real data (decimal severity, word-based severity, hours versus minutes, "all day", combination medications, brand names). Where a value is ambiguous, the literal stated information is preserved rather than an inferred total being calculated, since an incorrect inference about medication dosage carries real consequences.
Out of scope for this phase

AI-assisted resolution of low-confidence cases
Importing profile data (age, gender, cycle tracking); this remains part of onboarding
Calculating combined dose from quantity and per-unit dose
Structured symptom coding beyond existing trigger and notes fields
