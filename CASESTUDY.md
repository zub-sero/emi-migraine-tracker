Emi - Case Study
Emi (from emicrania, Italian for migraine) is designed to feel calm and approachable, not clinical.

The problem
Migraines are one of the most prevalent and debilitating neurological conditions in the world. Clinical guidance consistently recommends keeping a migraine diary to identify triggers, monitor frequency, and support treatment decisions. The tools available to do this are functional but limited: most are paper forms, exported spreadsheets from neurology department websites, or generic symptom trackers not built with migraine in mind. In practice, this means filling in a form at an appointment from memory, or maintaining a spreadsheet that requires discipline to update and effort to interpret.
I was inspired to build Emi because I have watched my wife suffer from chronic migraines for years. She has had them since childhood and has spent decades learning to manage a condition that, over time, clinical systems often stop actively trying to solve. The gap between what she needed and what existed was clear. The app she needed did not exist, so I built it.

The user
The primary user is a chronic migraine sufferer: someone who experiences migraines frequently enough to suspect patterns, who wants to understand their own condition better, and who needs a tool that works during a migraine, not just after one. They are often logging in low light, with limited cognitive bandwidth, wanting to do as little as possible to capture what is happening.

What I built
Emi is a progressive web app that lets users log migraines in two modes, track patterns over time, and export a clean PDF summary for their doctor.

Two logging modes with distinct purposes
"Migraine starting now" is designed for the moment the pain hits. One tap to start, a location selector, a severity slider, and a button to mark it as ended. Minimum friction, minimum reading, minimum decisions.
"Write it up" is for after recovery. A full structured form covering date, time of onset, duration, headache type, pain type, location, severity, triggers, and medication. Every field is optional except date.
A real-time entry is always editable after the fact. When someone recovers, they can return to the entry and expand it with the detail they could not manage during the migraine itself. Entries with missing detail show a clear prompt to complete them.

Medication logging as a data modelling decision
Most migraine tracking tools treat medication as a free text field. The result is that the same drug appears in the data as "Sumatriptan", "sumatriptan 50mg", "Suma", and a dozen other variations. Any pattern analysis built on that data is unreliable.
I designed a structured medication logging system with a curated list of common migraine medications as the primary input, autocomplete drawing from the user's own logging history, dose recorded as a standardised number with a unit dropdown (mg, mcg, ml, tablet), and time taken recorded per entry. Users can add medications not on the curated list via a clearly labelled free text option.
This decision was made with future aggregation and analysis in mind. A well-structured medication dataset is the difference between an app that shows you what you took and one that can tell you what works for you. That distinction matters if Emi ever moves beyond a single-user tool.

Insights powered by the Claude API
The insights page surfaces trigger frequency, time of day and day of week patterns, average severity and duration, and monthly frequency. An AI-generated summary reads across the user's entries and surfaces the most relevant observations in plain language, without requiring the user to interpret charts themselves.

Doctor export
A downloadable PDF summarising a selected time range. Total migraines, average severity, average duration, most common triggers, and a structured entry log including medications with dose, unit, and time taken. Built to be handed to a GP or neurologist without explanation.
Profile data
Users complete a short profile on first sign-in covering age range and gender. Women and non-binary users are also asked whether they want Emi to track cycle-based patterns. If yes, they are prompted to note their cycle phase when logging. This data enriches pattern analysis and lays the groundwork for hormonal correlation insights. Migraines affect women at roughly three times the rate of men, and hormonal triggers are among the most commonly reported. The cycle correlation feature in the insights layer is future scope, but the data architecture supports it from day one.

What I deliberately left out

Severity tracking across medication doses. A user taking three escalating doses during a single migraine could theoretically log severity at each point. The data would be clinically interesting. The logging experience would feel like a hospital intake form. A single severity-after-medication field was the right trade-off for the MVP.

Non-medication relief strategies. Rest, hydration, blood sugar management, and similar interventions are real and commonly reported. The logging model is built to accommodate this as a future field without structural changes, but adding it to the MVP would have expanded the form without enough corresponding value at this stage.

Migration from existing tracking systems. Many chronic migraine sufferers already have an established tracking history, often in a personal spreadsheet. The inability to import that history is a real adoption barrier. Mapping arbitrary spreadsheet structures to a standardised data model is a meaningful technical challenge and was out of scope for the MVP, but it is one of the first things worth solving if Emi moves toward wider use.

Scope beyond migraines. Emi does not try to solve headaches broadly, or chronic pain generally, or neurological conditions adjacent to migraine. This decision was made at the outset but its wisdom became clearer the deeper the build went. Migraines alone were hard enough to do well.

A healthcare professional view. The structured data model is designed to support a future professional-facing dashboard, but this was deliberately out of scope. The PDF export serves that relationship for now.

An administrator or research view. Aggregating data across users for population-level insights is a genuinely interesting product direction, and one with real market potential given how fragmented migraine data currently is. It is a separate product, not a feature, and building it before the consumer layer was solid would have been the wrong order.

What comes next

If Emi were to continue beyond an MVP portfolio project, the priorities would be:

User research with chronic migraine sufferers to validate the logging model and identify gaps, particularly around medication timing and the boundary between onset and pre-emptive dosing

Cycle correlation built into the insights layer for users who opt in, since hormonal triggers are among the most common and least well-documented

Push notification prompts to complete entries after a migraine ends, improving data completeness without requiring the user to remember

A migration tool allowing users to import existing tracking history from spreadsheets, reducing the switching cost for people who already have an established system

A healthcare professional view surfacing structured, consented data for clinicians or researchers

A population-level data layer for research partnerships, built on the standardised data model established in the MVP

Synthetic data testing of the analytics and reporting layer using fictional users with distinct migraine profiles reflecting known epidemiological patterns, to validate the insights and export features before a real user base generates sufficient data

Technical notes
Built in Lovable. Authentication and data storage via Supabase. AI-generated insights via the Anthropic Claude API. The app runs in the browser with no native installation required.
