Emi
A calm, focused migraine tracker for people who want to understand their condition better.
Live app: https://emi-migraine-tracker.lovable.app

What it does

Log migraines in real time with minimum taps, or write them up in full after recovery
Track triggers, medication, severity, duration, location, and headache type
View pattern insights powered by the Claude API: trigger frequency, time of day patterns, average severity, and monthly frequency
Export a clean PDF summary to share with a doctor or neurologist
Build a personal medication list with standardised dose and unit data for reliable pattern analysis


Key product decisions
Two logging modes with distinct purposes. "Migraine starting now" is built for the moment pain hits. "Write it up" is for after recovery. Both feed the same data model.
Medication logging as a data problem. Free text medication fields produce unreliable data. Emi uses a curated medication list with standardised dose and unit fields, user history autocomplete, and a free text fallback. This was a data modelling decision made with future aggregation in mind.
Structured for future use. The data architecture is designed to support a healthcare professional view and population-level research analysis. Neither is built yet. The foundation is there.

Built with

Lovable — front end and app scaffolding
Supabase — authentication and data storage
Anthropic Claude API — AI-generated insights


Known limitations and future scope

Cycle correlation data is collected but not yet surfaced in the insights layer
Non-medication relief strategies (rest, hydration, blood sugar management) are out of scope for this version
No import tool for users migrating from existing spreadsheet-based tracking systems
Synthetic data testing of the analytics layer is planned but not yet completed
A healthcare professional view and research dashboard are identified future directions


Case study
Full product decisions and trade-offs documented in CASESTUDY.md
