# Evidence Extension Migration Guide

## Overview

This guide helps developers and users adopt the GEDCOM Evidence Extension in their software and data.

## For Software Developers

### Basic Implementation Steps

1. **Parse Extension Tags**
   ```
   - Add _EVID record type
   - Add _RDOC record type  
   - Add _RACT record type
   - Add _EVID structure under INDI/FAM
   ```

2. **Data Model Updates**
   ```sql
   -- Example schema additions
   CREATE TABLE evidence (
     id VARCHAR(20) PRIMARY KEY,
     type VARCHAR(20),
     source_id VARCHAR(20) NOT NULL,
     created_date TIMESTAMP
   );
   
   CREATE TABLE evidence_identifiers (
     evidence_id VARCHAR(20),
     identifier TEXT,
     order_num INTEGER
   );
   
   CREATE TABLE person_evidence (
     person_id VARCHAR(20),
     evidence_id VARCHAR(20),
     confidence INTEGER CHECK (confidence >= 0 AND confidence <= 5),
     rdoc_id VARCHAR(20)
   );
   ```

3. **User Interface Considerations**
   - Evidence browser/manager
   - Confidence slider (0-5)
   - Research log interface
   - Analysis document editor

### Progressive Enhancement Approach

Start with basic support and add features incrementally:

**Phase 1: Read/Preserve**
- Parse extension records without losing data
- Display in raw form if needed
- Maintain for round-trip fidelity

**Phase 2: Basic Evidence**
- Create/edit _EVID records
- Link to persons with confidence
- Simple identifier interface

**Phase 3: Full Support**
- Research documentation
- Activity tracking
- GPS compliance features
- Evidence correlation tools

## For Users

### Converting Existing Data

#### From Research Logs

If you maintain research logs outside your genealogy software:

```gedcom
# Old approach: Research in notes
0 @I1@ INDI
1 NAME John /Smith/
1 NOTE Searched courthouse records Jan 2020, no birth record found

# New approach: Structured research activity
0 @R1@ _RACT
1 _TYPE Search
1 _TARG Birth record for John Smith
1 _VENU County Courthouse  
1 _RSLT NotFound
1 DATE JAN 2020
```

#### From Conflicting Sources

If you have conflicting information in notes:

```gedcom
# Old approach: Conflicts in notes
0 @I1@ INDI
1 NAME John /Smith/
1 BIRT
2 DATE ABT 1810
2 NOTE Census says 40 in 1850, death cert says 76 in 1885

# New approach: Separate evidence
0 @E1@ _EVID
1 _ID John Smith
1 _ID aged 40
1 SOUR @S1@  # 1850 Census

0 @E2@ _EVID  
1 _ID John Smith
1 _ID aged 76 years
1 SOUR @S2@  # Death cert

0 @I1@ INDI
1 NAME John /Smith/
1 _EVID @E1@
2 _CONF 3
1 _EVID @E2@
2 _CONF 3
```

#### From Uncertain Identities

If you have "possible" or "probably" notes:

```gedcom
# Old approach: Uncertainty in notes
0 @I1@ INDI
1 NAME John /Smith/
1 NOTE Might be the John Smith in 1850 census

# New approach: Evidence with confidence
0 @E1@ _EVID
1 _ID John Smith
1 _ID head of household
1 SOUR @S1@

0 @I1@ INDI
1 NAME John /Smith/
1 _EVID @E1@
2 _CONF 3  # 60% confident
```

### Best Practices

1. **Start Small**
   - Begin with obvious evidence extractions
   - Add confidence levels to existing sources
   - Document one research project fully

2. **Be Consistent**
   - Use consistent identifier formats
   - Apply confidence scale uniformly
   - Document your methodology

3. **Think Evidence-First**
   - Extract what sources say, not what you conclude
   - Keep identifiers close to source text
   - Let evidence float until certain

## For GEDCOM Files

### Header Declaration

Add extension declaration to file header:

```gedcom
0 HEAD
1 GEDC
2 VERS 7.0
1 SCHMA
2 TAG _EVID https://github.com/genealogy-standards/gedcom-evidence/v1/EVID
2 TAG _RDOC https://github.com/genealogy-standards/gedcom-evidence/v1/RDOC
2 TAG _RACT https://github.com/genealogy-standards/gedcom-evidence/v1/RACT
```

### Backward Compatibility

Files using this extension remain readable by software that doesn't support it:

1. Unknown records (_EVID, _RDOC, _RACT) are ignored
2. Unknown structures under INDI/FAM are preserved
3. Standard GEDCOM structures unchanged
4. Source citations remain standard

## Common Patterns

### Pattern 1: Census Household

```gedcom
# Extract each person mentioned
0 @E1@ _EVID
1 _ID John Smith
1 _ID head
1 SOUR @S1@

0 @E2@ _EVID
1 _ID Mary Smith  
1 _ID wife
1 SOUR @S1@

# Link with confidence
0 @I1@ INDI
1 NAME John /Smith/
1 _EVID @E1@
2 _CONF 5
```

### Pattern 2: Identity Correlation

```gedcom
# Multiple evidence for same person
0 @R1@ _RDOC
1 _TITL Proving John Smith identity
1 _TEXT Evidence correlation shows...
1 _EVID @E1@  # Birth record
1 _EVID @E2@  # Marriage record
1 _EVID @E3@  # Death record
1 _CONC
2 _SUBJ @I1@
2 _TEXT All three records refer to same person
```

### Pattern 3: Research Planning

```gedcom
# Document research strategy
0 @R1@ _RACT
1 _TYPE Analysis
1 _PURP Create research plan for Smith family
1 _PLAN Need to search:
2 CONT 1. Vital records
2 CONT 2. Census records  
2 CONT 3. Probate records
1 DATE 1 JAN 2025
```

## Validation

### Required Elements

Ensure your evidence records have:
- At least one identifier (_ID)
- At least one source (SOUR)
- Appropriate type if specified

### Confidence Levels

Apply consistently:
- 0-1: Probably not this person
- 2-3: Possibly this person
- 4-5: Probably/definitely this person

### Documentation

For GPS compliance, ensure:
- Exhaustive searches documented in _RACT
- Analysis documented in _RDOC
- Conflicts addressed explicitly
- Conclusions clearly stated

## Getting Help

- Review [use-cases.md](use-cases.md) for examples
- Check [specification.md](specification.md) for technical details
- Consult [glossary.md](glossary.md) for terminology
- Join community discussions at [project forum]

## Summary

The Evidence Extension enhances GEDCOM without breaking it. Start small, focus on real problems you face, and gradually adopt more features as needed. The goal is better genealogy through better evidence handling.