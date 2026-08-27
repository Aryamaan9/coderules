# AI & HUMAN CODING STANDARDS

**1. MODULARITY & HYGIENE**
- **Isolation:** Keep features logically isolated so that if something breaks, only one feature is affected.
- **Single Source of Truth:** Important business rules, formulas, and calculations must have one authoritative implementation. Do not create competing versions of the same logic in different parts of the application.

**2. DATA & SECURITY**
- **Authoritative Security:** Enforce security and validation at the backend/database layer, never just in the UI.
- **Migrations:** All database schema changes require versioned, reproducible migration files.
- **Secrets:** Never store secrets in frontend code, Git, or logs.

**3. DOCUMENTATION**
- **Business Logic:** Important business rules, assumptions, methodologies, and calculation definitions must be documented independently of the implementation where they could otherwise be misunderstood.
- **Feature Docs:** Meaningful features require documentation explaining workflows, business rules, and dependencies.
- **Code Comments:** Explain *WHY* for non-obvious code, complex calculations, security decisions, and workarounds.

**4. VERIFICATION & TESTING**
- **Real Workflows:** Test the actual user workflow and data persistence, not just whether the UI renders.
- **Beyond Happy Path:** Test important validation, permission, failure, and edge-case paths where relevant, not only the normal successful workflow.
- **Calculations:** Financial or complex numerical logic must be independently verified against edge cases.
