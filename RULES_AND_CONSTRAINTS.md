# GEDCOM Evidence Extension Rules and Constraints

## Overview

The GEDCOM Evidence Extension v0.2.0 provides a GEDCOM 7-compliant way to represent evidence in genealogical data. This extension follows a dual-pattern approach that works within GEDCOM's strict structural constraints.

## What This Extension IS

1. **A way to store evidence as floating records** - Evidence can exist independently and be referenced by multiple individuals
2. **A method to attach evidence directly to individuals** - For evidence that definitively belongs to one person
3. **GEDCOM 7 compliant** - Follows all pointer, record, and substructure rules
4. **GPS-aligned** - Supports the Genealogical Proof Standard's evidence-first methodology

## What This Extension IS NOT

1. **NOT a way to point from evidence to multiple record types** - GEDCOM forbids polymorphic pointers
2. **NOT able to create bidirectional relationships** - Evidence cannot point back to individuals
3. **NOT able to override standard GEDCOM structures** - Extensions cannot change existing meanings
4. **NOT a replacement for source citations** - This complements, not replaces, SOUR structures

## Core GEDCOM Rules This Extension Follows

### 1. No Polymorphic Pointers
❌ **NOT ALLOWED**: `_ID @I1@|@F1@|@S1@` (pointing to INDI OR FAM OR SOUR)  
✅ **ALLOWED**: `_EVREF @E1@` (pointing only to _EVID records)

### 2. Records vs Substructures
- **Records** (level 0): Can have cross-reference identifiers (@ID@), can be pointed to
- **Substructures** (level 1+): Cannot have IDs, cannot be pointed to

❌ **NOT ALLOWED**: Pointing to a substructure within a record  
✅ **ALLOWED**: Pointing only to complete records

### 3. One-Way References Only
❌ **NOT ALLOWED**: Evidence records containing pointers to individuals  
✅ **ALLOWED**: Individuals containing pointers to evidence records

### 4. Proper Payload Formats
❌ **NOT ALLOWED**: `payload: XREF` or `payload: https://gedcom.io/terms/v7/type-Xref`  
✅ **ALLOWED**: `payload: "@<https://github.com/glamberson/gedcom-evidence/record-EVID>@"`

## The Two Patterns

### Pattern 1: Shadow Records (Floating Evidence)
- Evidence exists as `_EVID` records at level 0
- Individuals reference evidence using `_EVREF` substructures
- Multiple individuals can reference the same evidence
- Evidence contains findings but no identity conclusions

**Example**:
```gedcom
0 @E1@ _EVID
1 _DTYPE Census
1 _FIND John Smith, age 42
2 TYPE Name and Age

0 @I1@ INDI
1 _EVREF @E1@
2 _CONF High
```

### Pattern 2: Event Container (Attached Evidence)
- Evidence attached directly to an individual as `_EVEN_EVID`
- Used when evidence definitively belongs to this person
- No floating record needed

**Example**:
```gedcom
0 @I1@ INDI
1 _EVEN_EVID
2 TYPE Evidence Discovery
2 _FIND Birth certificate for John Smith
```

## What You CAN Do

1. ✅ Create evidence records that multiple people can reference
2. ✅ Store exact findings from sources without forcing identity decisions
3. ✅ Attach confidence assessments to evidence-person connections
4. ✅ Document how evidence was used in your research
5. ✅ Mix both patterns in the same file

## What You CANNOT Do

1. ❌ Make evidence records point to individuals/families
2. ❌ Create circular references between records
3. ❌ Point to parts of records (only whole records)
4. ❌ Use one structure to point to different record types
5. ❌ Override or extend standard GEDCOM structures

## Technical Constraints

### Valid Pointers
- `_EVREF` can ONLY point to `_EVID` records
- No structure can point to INDI AND FAM AND SOUR
- All pointers must use proper URI syntax in payloads

### Structure Hierarchy
- `_EVID` is a record (level 0)
- `_EVREF`, `_FIND`, `_CONF`, etc. are substructures (level 1+)
- Only records can be targets of pointers

### Cardinality Rules
- Most substructures allow {0:M} (zero to many)
- Some allow {0:1} (zero or one)
- Extensions cannot change existing cardinalities

## Why These Constraints Exist

GEDCOM's constraints ensure:
1. **Data integrity** - No ambiguous references
2. **Parser simplicity** - Predictable structure
3. **Backward compatibility** - Extensions don't break existing software
4. **Type safety** - Each pointer has one clear target type

By working within these constraints, our extension remains fully compliant and interoperable with all GEDCOM 7-supporting software.