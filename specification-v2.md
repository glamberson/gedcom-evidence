# GEDCOM Evidence Extension v2.0 Specification

## Overview

The GEDCOM Evidence Extension provides structures for representing genealogical evidence as distinct from conclusions. This extension implements the evidence-first methodology recommended by the Genealogical Proof Standard (GPS) and enables:

1. **Floating Evidence**: Evidence that can be associated with multiple potential individuals
2. **Evidence Preservation**: Exact recording of what sources say without forcing conclusions
3. **Research Process Documentation**: Tracking how evidence supports or conflicts with conclusions
4. **Identity Resolution**: Managing uncertain identities until research resolves them

## Design Philosophy

This extension uses a dual-pattern approach to work within GEDCOM 7's constraints:

1. **Pattern 1: Shadow Records** - Evidence exists as independent records (_EVID) that can be referenced by multiple individuals through lightweight reference structures (_EVREF)

2. **Pattern 2: Event Containers** - Evidence directly tied to a specific individual can be recorded as evidence events (_EVEN_EVID)

## Core Structures

### _EVID (Evidence Container Record)

A top-level record representing a piece of evidence as an independent object.

```
0 @E1@ _EVID
  +1 _DTYPE <Text>               {0:1}    # Document type
  +1 DATE <DateValue>            {0:1}    # When evidence was found/created
  +1 <<SOURCE_CITATION>>         {1:M}    # Required: what source
  +1 _FIND <Text>                {0:M}    # Findings extracted
     +2 TYPE <Text>              {1:1}    # Type of finding
     +2 DATE <DateValue>         {0:1}    # Date found in evidence
     +2 PLAC <PlaceValue>        {0:1}    # Place found in evidence
     +2 AGE <Age>                {0:1}    # Age found in evidence
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _ANAL <Text>                {0:M}    # Analysis notes
     +2 DATE <DateValue>         {0:1}    # When analyzed
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _SUBJ_INDI @<XREF:INDI>@   {0:M}    # Individual subjects
     +2 ROLE <Text>              {0:1}    # Role in evidence
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _SUBJ_FAM @<XREF:FAM>@     {0:M}    # Family subjects
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _SUBJ_SOUR @<XREF:SOUR>@   {0:M}    # Source subjects
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 <<NOTE_STRUCTURE>>          {0:M}
  +1 <<CHANGE_DATE>>             {0:1}
  +1 <<CREATION_DATE>>           {0:1}
  +1 UID <UID>                   {0:M}
```

### _EVREF (Evidence Reference)

References evidence from within an individual or family record.

```
n _EVREF @<XREF:EVID>@           {0:M}
  +1 _CONF <Enum>                {0:1}    # Confidence level
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _USED <Text>                {0:1}    # How evidence was used
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 QUAY <Enum>                 {0:1}    # Quality of data
  +1 <<NOTE_STRUCTURE>>          {0:M}
```

Superstructures: INDI, FAM

### _EVEN_EVID (Evidence Event)

An event representing evidence discovery directly tied to an individual.

```
n _EVEN_EVID                     {0:M}
  +1 TYPE <Text>                 {1:1}    # Always "Evidence Discovery"
  +1 DATE <DateValue>            {0:1}    # When found/analyzed
  +1 <<SOURCE_CITATION>>         {1:M}    # Required: what source
  +1 _FIND <Text>                {0:M}    # Findings
     +2 TYPE <Text>              {1:1}
     +2 DATE <DateValue>         {0:1}
     +2 PLAC <PlaceValue>        {0:1}
     +2 AGE <Age>                {0:1}
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 _CONF <Enum>                {0:1}    # Confidence
     +2 <<NOTE_STRUCTURE>>       {0:M}
  +1 <<NOTE_STRUCTURE>>          {0:M}
```

Superstructures: INDI

## Supporting Structures

### _DTYPE (Document Type)

Specifies the type of document containing the evidence.

- **Payload**: Text
- **Examples**: "Census", "Birth Certificate", "Marriage License", "Will", "Land Record"
- **Superstructures**: _EVID

### _FIND (Evidence Finding)

A specific fact extracted from evidence, preserving exact text.

- **Payload**: Text (the exact finding)
- **Required substructure**: TYPE (what kind of finding)
- **Optional substructures**: DATE, PLAC, AGE, NOTE
- **Superstructures**: _EVID, _EVEN_EVID

### _ANAL (Analysis)

Researcher's analysis and interpretation of evidence.

- **Payload**: Text
- **Optional substructures**: DATE, NOTE
- **Superstructures**: _EVID, _EVREF

### _CONF (Confidence)

Confidence level in the evidence-to-conclusion connection.

- **Payload**: Enumeration
- **Values**: High | Medium | Low | Hypothesis
- **Superstructures**: _EVREF, _EVEN_EVID

### _USED (How Used)

Documents how evidence supports conclusions.

- **Payload**: Text
- **Superstructures**: _EVREF

### _SUBJ_INDI, _SUBJ_FAM, _SUBJ_SOUR

Reference structures pointing to subjects mentioned in evidence. Three separate structures avoid polymorphic pointers.

## Usage Patterns

### Pattern 1: Floating Evidence

Use when evidence could apply to multiple individuals:

```gedcom
0 @E1@ _EVID
1 _DTYPE Census
1 DATE 7 JUN 1850
1 SOUR @S1@
2 PAGE Line 15
1 _FIND John Smith, age 42
2 TYPE Name and Age
1 _SUBJ_INDI @I1@
2 ROLE Possible match
1 _SUBJ_INDI @I2@
2 ROLE Alternative match

0 @I1@ INDI
1 NAME John /Smith/
1 _EVREF @E1@
2 _CONF High
2 _USED Age matches exactly

0 @I2@ INDI
1 NAME John /Smith/
1 _EVREF @E1@
2 _CONF Low
2 _USED Age differs by 5 years
```

### Pattern 2: Attached Evidence

Use when evidence is definitively about one individual:

```gedcom
0 @I1@ INDI
1 NAME Sarah /Jones/
1 _EVEN_EVID
2 TYPE Evidence Discovery
2 DATE 15 MAR 2024
2 SOUR @S1@
3 PAGE Image 1234
2 _FIND Sarah Jones born 4 May 1823
3 TYPE Birth Record
2 _CONF High
```

## Best Practices

1. **Preserve Original Text**: Use _FIND to record exact text from sources
2. **Document Analysis**: Use _ANAL to explain reasoning
3. **Track Confidence**: Always specify _CONF levels
4. **Explain Usage**: Use _USED to document how evidence supports conclusions
5. **Float When Uncertain**: Use Pattern 1 when identity is unclear
6. **Attach When Certain**: Use Pattern 2 for definitive evidence

## Schema Declaration

```gedcom
0 HEAD
1 SCHMA
2 TAG _EVID https://github.com/glamberson/gedcom-evidence/_EVID
2 TAG _EVREF https://github.com/glamberson/gedcom-evidence/_EVREF
2 TAG _EVEN_EVID https://github.com/glamberson/gedcom-evidence/_EVEN_EVID
2 TAG _FIND https://github.com/glamberson/gedcom-evidence/_FIND
2 TAG _DTYPE https://github.com/glamberson/gedcom-evidence/_DTYPE
2 TAG _ANAL https://github.com/glamberson/gedcom-evidence/_ANAL
2 TAG _CONF https://github.com/glamberson/gedcom-evidence/_CONF
2 TAG _USED https://github.com/glamberson/gedcom-evidence/_USED
2 TAG _SUBJ_INDI https://github.com/glamberson/gedcom-evidence/_SUBJ_INDI
2 TAG _SUBJ_FAM https://github.com/glamberson/gedcom-evidence/_SUBJ_FAM
2 TAG _SUBJ_SOUR https://github.com/glamberson/gedcom-evidence/_SUBJ_SOUR
```

## Version History

- **v2.0** (August 2025): Complete redesign to comply with GEDCOM 7 constraints
  - Eliminated polymorphic pointers
  - Introduced dual-pattern approach
  - Added separate subject reference structures
- **v0.1** (2024): Initial design (withdrawn due to rule violations)

## Contact

Greg Lamberson <lamberson@yahoo.com>