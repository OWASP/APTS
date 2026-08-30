# OWASP Autonomous Penetration Testing Standard (APTS)

## Frontispiece

---

## Project Information

| Attribute | Value |
|-----------|-------|
| **Project Name** | OWASP Autonomous Penetration Testing Standard (APTS) |
| **Version** | 0.1.0 |
| **Release Date** | April 2026 |
| **License** | Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) |
| **Repository** | https://github.com/OWASP/APTS |
| **Contact** | jinson@owasp.org |

---

## Project Leadership

- **Jinson Varghese Behanan**
- **Shikhil Sharma**

## Contributors

See [ACKNOWLEDGEMENTS.md](../ACKNOWLEDGEMENTS.md) for the full list of contributors.

---

## Document Conventions

This standard uses RFC 2119 language:

| Term | Meaning |
|------|---------|
| **MUST** | Mandatory requirement. No deviation is permitted for a tier claim. |
| **MUST NOT** | Absolute prohibition. |
| **SHOULD** | Recommended. Deviation requires documented justification. |
| **SHOULD NOT** | Not recommended. Deviation is acceptable with documented justification. |
| **MAY** | Optional. Implementation is at the organization's discretion. |
| **OPTIONAL** | Truly discretionary. May be included based on organizational needs. |

A requirement's classification (MUST or SHOULD, shown in its Requirement Index entry) sets its conformance weight for the tier. Within a requirement's text, sub-clauses may carry their own RFC 2119 keywords describing how the control is implemented. Where a SHOULD-classified requirement contains MUST sub-clauses, those sub-clauses are conditional: they become mandatory only once the organization elects to implement the requirement (or where the requirement's Applicability statement is met). A SHOULD requirement may still be deviated from with documented justification per the conformance rules in the Introduction.

---

## Requirement ID Format

Requirements follow the format: **APTS-[DOMAIN]-[NUMBER]**

| Domain | Prefix | Section |
|--------|--------|---------|
| Scope Enforcement | SE | 1 |
| Safety Controls & Impact Management | SC | 2 |
| Human Oversight & Intervention | HO | 3 |
| Graduated Autonomy Levels | AL | 4 |
| Auditability & Reproducibility | AR | 5 |
| Manipulation Resistance | MR | 6 |
| Third-Party & Supply Chain Trust | TP | 7 |
| Reporting | RP | 8 |

---

## Copyright and License

Copyright 2026 The OWASP Foundation.

Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). You may share and adapt this material for any purpose, including commercial, provided you give attribution and distribute under the same license.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| 0.1.0 | April 2026 | Initial release. Eight domains, 173 tier-required requirements across three compliance tiers, plus 20 advisory practices in the appendix. |

