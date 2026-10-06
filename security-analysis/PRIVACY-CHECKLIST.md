# Privacy checklist (GDPR / personopplysningsloven)

Used by [SKILL.md](SKILL.md) step 5. Based on the requirements Datatilsynet enforces.
Check each item and note the status (OK / Deviation / N/A) with a short reason.
Clear breaches are raised as findings in the main report with priority **Critical**
or **High**.

## Legal basis

- Is it documented which basis (consent, contract, legal authority, legitimate
  interest) each processing activity rests on?
- For consent: is it freely given, specific, informed and easy to withdraw?

## Data minimization

- Are more fields collected than the purpose requires?
- Is personal data (email, national ID number, IP, location) logged when it does not need to be?
- Is real personal data used in test or dev environments?

## Storage limitation

- Are there defined deletion rules or anonymization, or only indefinite storage?
- Is data also deleted from backups, search indexes, logs and caches?

## Privacy by design

- Are sensitive fields encrypted at rest (database, files, backup)?
- Is data pseudonymized where possible (hashing, tokenization)?
- Are default settings the most privacy-friendly choices?

## Data subject rights

- Are there mechanisms for access, rectification, erasure and data portability?
- Can all data about one person be exported without manual digging?

## Third-party transfer

- Is data sent to countries outside the EU/EEA?
- If so: is there a valid transfer basis (adequacy decision, SCC, BCR)?
- Are subcontractors (data processors) listed and covered by a data processing agreement?

## Breach notification

- Is there a routine to detect breaches and notify Datatilsynet within 72 hours?
- Is it defined who the controller is and who the contact point is?

## DPIA (data protection impact assessment)

- Does the processing require a DPIA (high risk, special categories, large volumes)?
- If so: does it exist, and is it up to date with the current implementation?