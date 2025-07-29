# GEDCOM Evidence Extension

A GEDCOM 7-compliant extension for evidence-based genealogy, supporting the Genealogical Proof Standard (GPS) through dual patterns that work within GEDCOM's structural constraints.

## Version

0.2.0 - Complete redesign for GEDCOM 7 compliance

## Overview

This extension provides two complementary patterns for handling evidence in genealogical research:

1. **Shadow Records Pattern** - Floating evidence that can be referenced by multiple individuals
2. **Event Container Pattern** - Evidence attached directly to specific individuals

Both patterns work within GEDCOM 7's rules while supporting evidence-first research methodologies.

### Key Features

- **GEDCOM 7 Compliant** - Follows all pointer, record, and substructure rules
- **Evidence Containers** - Store source information exactly as found
- **Flexible References** - Individuals can reference shared evidence
- **Confidence Assessment** - Track your confidence in evidence connections
- **GPS Support** - Aligns with professional genealogical standards

## Why This Redesign?

Version 0.1.x attempted polymorphic pointers and bidirectional references that violate core GEDCOM rules. Version 0.2.0 works WITH GEDCOM's constraints for a cleaner, compliant design.

## Installation

1. GEDCOM 7-compatible software should automatically recognize these extension tags
2. The YAML files follow the official GEDCOM registry format
3. All structures are designed to integrate seamlessly with standard GEDCOM

## The Two Patterns

### Pattern 1: Shadow Records (Floating Evidence)

Use this when evidence might apply to multiple people or when identity is uncertain.

```gedcom
0 @E1@ _EVID
1 _DTYPE Census
1 DATE 7 JUN 1850
1 SOUR @S1@
2 PAGE Lines 15-16
1 _FIND John Smith, age 42, farmer
2 TYPE Personal Information
1 _FIND Mary Smith, age 38
2 TYPE Spouse Information

0 @I1@ INDI
1 NAME John /Smith/
1 _EVREF @E1@
2 _CONF High
2 _USED Name and age match other records

0 @I99@ INDI
1 NAME John /Smith/
1 _EVREF @E1@
2 _CONF Low
2 _USED Age differs by 5 years
```

### Pattern 2: Event Container (Attached Evidence)

Use this when evidence definitively belongs to a specific person.

```gedcom
0 @I1@ INDI
1 NAME Sarah /Jones/
1 _EVEN_EVID
2 TYPE Evidence Discovery
2 DATE 15 MAR 2024
2 SOUR @S5@
3 PAGE Image 1234
2 _FIND Sarah Jones born 4 May 1823
3 TYPE Birth Information
2 _CONF High
```

## Extension Structures

- **_EVID** - Evidence Container Record (level 0)
- **_EVREF** - Evidence Reference (points from person to evidence)
- **_FIND** - Specific finding within evidence
- **_EVEN_EVID** - Evidence as an event
- **_CONF** - Confidence level (High/Medium/Low/Hypothesis)
- **_USED** - How evidence was used
- **_DTYPE** - Document type
- **_ANAL** - Analysis notes

## Key Benefits

1. **Preserves Original Evidence** - Never lose what sources actually said
2. **Supports Uncertain Identity** - Evidence can exist independently
3. **GEDCOM Compliant** - Works within all structural rules
4. **GPS Aligned** - Supports professional research standards
5. **Flexible Application** - Use one or both patterns as needed

## Documentation

- [RULES_AND_CONSTRAINTS.md](RULES_AND_CONSTRAINTS.md) - What is and isn't allowed
- [Design Rationale](design-rationale.md) - Why these design choices
- [Migration Guide](migration-guide.md) - Upgrading from v0.1.x
- [Use Cases](use-cases.md) - Real-world examples
- [Examples](examples/) - Sample GEDCOM files

## Community Foundation

This extension builds on 15 years of community discussion, particularly:
- BetterGEDCOM project insights (2010-2013)
- GEDCOM 7 development feedback
- Professional genealogist requirements

## Status

**Version 0.2.0** - Complete redesign addressing fundamental GEDCOM compliance issues

**Previous PR** - v0.1.x PR #178 was closed due to rule violations

**Next Steps** - New PR to be submitted with compliant design

## Contributing

This extension is open for community input. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

Released under the same terms as GEDCOM 7 specifications.