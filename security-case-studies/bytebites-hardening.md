# ByteBites Hardening Case Study

## Introduction
ByteBites is a Python backend modeling customers, products, transactions, and purchase history for a restaurant-style ordering system. The project needed to implement the object model and business logic cleanly, but I also used it to tighten validation, bound resource usage, protect internal shared state, and make monetary calculations exact and reproducible.

The main hardening work focused on centralized allowlist validation, explicit size limits, safer handling of shared internal structures, and replacing float-based transaction totals with `Decimal` so the system would behave predictably under malformed input and edge-case arithmetic.

## Threats / Abuse Cases Considered
The main risks considered were:

- malformed text or numeric input being accepted into the model
- unbounded collection growth causing memory pressure (availability risk)
- callers mutating returned internal state in ways that break invariants
- inaccurate monetary totals caused by floating-point behavior

These map directly to model-layer changes: strict text validation, numeric normalization, resource limits, immutable catalog return values, and `Decimal`-based totals.

## Key Themes
- **Boundaries:** Validation and normalization happen at object construction rather than being deferred to calling code.
- **Validation:** Text, numeric fields, and type expectations are enforced with centralized helpers.
- **Internal State Treated Carefully:** Shared collections are exposed more safely so outside code cannot casually mutate important structures.
- **Data Integrity:** Exact arithmetic, predictable object behavior, and stable internal state.
- **Tests as Verification:** Security-relevant changes (especially `Decimal` and validation behavior) are reflected in the test suite.
- **Framework Mapping:** Most aligned to OWASP A03/A04, CWE-20, CWE-681, CWE-682, and NIST SI-10.

## Key Decisions

### Centralized allowlist validation
Loose, inconsistent field checks were replaced with centralized validation helpers. Text fields are stripped, length-limited, type-checked, and validated against an allowlist regex, blocking malformed or unexpected input before it is stored.

**Evidence:** `models.py` (`_sanitize_text`, `_SAFE_TEXT_RE`, `Products.__post_init__`, `Customers.__post_init__`)

### Exact arithmetic for monetary values
The original implementation used float arithmetic for transaction totals. I replaced this with `Decimal`-based calculation and a `_to_decimal()` helper to normalize numeric input safely. This removed floating-point precision drift and made totals reproducible.

**Evidence:** `models.py` (`_to_decimal`, `Transactions.calculateTotalCost`, `Products.__post_init__`)

### Internal shared state was exposed more safely
`Products.listCatalog()` was changed to return an immutable tuple rather than a mutable list. This prevents casual external mutation of the returned container and makes intended read-only usage clearer.

**Evidence:** `models.py` (`Products.listCatalog`)

### Resource usage was bounded
The model enforces explicit caps on catalog size, transaction size, purchase-history size, and text-field lengths. These limits reduce the chance of uncontrolled in-memory growth and keep behavior predictable.

**Evidence:** `models.py` (`MAX_NAME_LENGTH`, `MAX_CATEGORY_LENGTH`, `MAX_CATALOG_SIZE`, `MAX_TRANSACTION_ITEMS`, `MAX_PURCHASE_HISTORY`, `Products.__post_init__`, `Transactions.addItem`, `PurchaseHistory.addTransaction`)

## Verification
The test suite and sanity checks were updated to verify hardened behavior, including:

- `Decimal`-based transaction totals
- rejection of invalid input
- correct transaction behavior and filtering
- consistency between implementation behavior and updated expected results

**Evidence:** `test_bytebites.py`; `tmp_sanity_check.py`

## Reproduction
```bash
python -m pytest test_bytebites.py -v
python tmp_sanity_check.py
```

Expected results:
- pytest: **7 passed, 0 failed**
- sanity check: `All sanity checks passed.`

## Framework Mapping (high level)
- **OWASP A03:2021 (Injection):** allowlist validation on stored text fields
- **OWASP A04:2021 (Insecure Design):** resource limits; safer handling of returned internal structures; exact arithmetic as a design requirement
- **CWE-20 (Improper Input Validation):** strict type, length, and character checks
- **CWE-681 / CWE-682:** safer numeric conversion and correct monetary calculation
- **NIST SP 800-53 SI-10 (Information Input Validation):** validation prior to processing or storage

## Useful Links
- ByteBites repository: https://github.com/JasmineSutton/ByteBites
- SECURITY_README.md: https://github.com/JasmineSutton/ByteBites/blob/7a449b0dfc703fabd62696536e602093e0feb620/SECURITY_README.md
