# Registry-Compliant YAML Files

This directory contains the GEDCOM registry-compliant YAML files for the gedcom-evidence extension.

These files were submitted to the official GEDCOM registry via PR #173:
https://github.com/FamilySearch/GEDCOM-registries/pull/173

## Structure

The files follow the official GEDCOM registry structure:

- `record/` - Record type definitions (record-EVID, record-_RDOC)
- `structure/` - Extension tag definitions (_EVID as substructure, _FIND, etc.)
- `enumeration/` - Individual enumeration values (High, Medium, Low, Hypothesis)
- `enumeration-set/` - Enumeration set definition (Confidence)

## Key Differences from Original yaml/ Directory

The original `yaml/` directory files had several issues that prevented registry acceptance:
- Used `type: record` instead of `type: structure`
- Missing enumeration definitions
- Incorrect enumeration value format (lowercase instead of uppercase)

These registry-compliant versions have been validated and accepted by the GEDCOM registry validator.

## Usage

These files are the authoritative definitions for the gedcom-evidence extension tags.
Applications implementing this extension should reference these definitions.