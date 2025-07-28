# GEDCOM Evidence Extension Specification v1.0

## Overview

This specification defines three new record types and associated structures for evidence-based genealogy in GEDCOM 7.

## Extension Namespace

```
URI: https://github.com/genealogy-standards/gedcom-evidence/v1
Tag Prefix: (none - uses underscore convention)
```

## Record Types

### 1. Evidence Container (_EVID)

**Purpose**: Store information extracted from sources using identifiers.

**Structure**:
```gedcom
0 @<XREF>@ _EVID
1 _TYPE <TYPE_ENUM>             {0:1}
1 _ID <Text>                    {1:M}
1 SOUR @<XREF:SOUR>@           {1:M}
  +2 <<SOURCE_CITATION>>
1 NOTE <Text>                   {0:M}
  +2 <<NOTE_STRUCTURE>>
1 CHAN                          {0:1}
  +2 <<CHANGE_DATE>>
```

**Enumerations**:
- TYPE_ENUM: Person|Event|Relationship|Place|Other

### 2. Research Documentation (_RDOC)

**Purpose**: Document analysis, reasoning, and proof arguments.

**Structure**:
```gedcom
0 @<XREF>@ _RDOC
1 _TITL <Text>                  {0:1}
1 _TEXT <Text>                  {1:1}
  +2 CONT <Text>                {0:M}
1 _SUBJ @<XREF>@               {0:M}
1 _EVID @<XREF:_EVID>@         {0:M}
1 _CONC                         {0:M}
  +2 _SUBJ @<XREF>@            {1:1}
  +2 _TEXT <Text>               {1:1}
1 AUTH <Text>                   {0:1}
1 DATE <DateValue>              {0:1}
  +2 TIME <Time>                {0:1}
1 NOTE <Text>                   {0:M}
  +2 <<NOTE_STRUCTURE>>
1 CHAN                          {0:1}
  +2 <<CHANGE_DATE>>
```

### 3. Research Activity (_RACT)

**Purpose**: Track research activities and searches.

**Structure**:
```gedcom
0 @<XREF>@ _RACT
1 _TYPE <ACTIVITY_TYPE>         {1:1}
1 _PURP <Text>                  {0:1}
1 _TARG <Text>                  {0:1}
1 _VENU <Text>                  {0:1}
1 _RSLT <RESULT_ENUM>           {0:1}
1 _FIND <Text>                  {0:M}
  +2 _EVID @<XREF:_EVID>@      {0:M}
1 DATE <DateValue>              {1:1}
  +2 TIME <Time>                {0:1}
1 _RSRCH <Text>                 {0:1}
1 NOTE <Text>                   {0:M}
  +2 <<NOTE_STRUCTURE>>
1 CHAN                          {0:1}
  +2 <<CHANGE_DATE>>
```

**Enumerations**:
- ACTIVITY_TYPE: Search|Review|Analysis|Correspondence
- RESULT_ENUM: Found|NotFound|Partial|Pending

## Structure Extensions

### Evidence Reference (for INDI/FAM records)

**Purpose**: Link persons/families to supporting evidence.

**Structure**:
```gedcom
n _EVID @<XREF:_EVID>@         {0:M}
  +1 _CONF <Integer>            {0:1}
  +1 _RDOC @<XREF:_RDOC>@      {0:M}
  +1 NOTE <Text>                {0:M}
    +2 <<NOTE_STRUCTURE>>
```

**Value Constraints**:
- _CONF: Integer 0-5 (0=no confidence, 5=certain)

## YAML Definitions

### _EVID.yaml
```yaml
%YAML 1.2
---
lang: en-US
type: record
uri: https://github.com/genealogy-standards/gedcom-evidence/v1/EVID

label: Evidence Container
specification: |
  A container for information extracted from a source. Uses identifiers
  to describe subjects without making conclusions about specific persons.
  Evidence can float unattached or be associated with multiple persons
  with different confidence levels.

payload: null

substructures:
  - _TYPE: "{0:1}"
  - _ID: "{1:M}"
  - SOUR: "{1:M}"
  - NOTE: "{0:M}"
  - CHAN: "{0:1}"

superstructures: []
```

### _ID.yaml
```yaml
%YAML 1.2
---
lang: en-US
type: structure
uri: https://github.com/genealogy-standards/gedcom-evidence/v1/ID

label: Identifier
specification: |
  Text from a source that identifies the subject of evidence.
  Multiple identifiers provide context for identity resolution.
  Examples: names, relationships, ages, occupations, descriptions.

payload: Text

substructures: []

superstructures:
  - _EVID
```

### _RDOC.yaml
```yaml
%YAML 1.2
---
lang: en-US
type: record
uri: https://github.com/genealogy-standards/gedcom-evidence/v1/RDOC

label: Research Documentation
specification: |
  Documents analysis, reasoning, and proof arguments. Supports
  Genealogical Proof Standard compliance by recording correlation
  of evidence and conclusions reached.

payload: null

substructures:
  - _TITL: "{0:1}"
  - _TEXT: "{1:1}"
  - _SUBJ: "{0:M}"
  - _EVID: "{0:M}"
  - _CONC: "{0:M}"
  - AUTH: "{0:1}"
  - DATE: "{0:1}"
  - NOTE: "{0:M}"
  - CHAN: "{0:1}"

superstructures: []
```

### _RACT.yaml
```yaml
%YAML 1.2
---
lang: en-US
type: record
uri: https://github.com/genealogy-standards/gedcom-evidence/v1/RACT

label: Research Activity
specification: |
  Records a specific research action including searches, reviews,
  and analysis. Captures both positive and negative results.
  Essential for documenting exhaustive searches per GPS.

payload: null

substructures:
  - _TYPE: "{1:1}"
  - _PURP: "{0:1}"
  - _TARG: "{0:1}"
  - _VENU: "{0:1}"
  - _RSLT: "{0:1}"
  - _FIND: "{0:M}"
  - DATE: "{1:1}"
  - _RSRCH: "{0:1}"
  - NOTE: "{0:M}"
  - CHAN: "{0:1}"

superstructures: []
```

### _CONF.yaml
```yaml
%YAML 1.2
---
lang: en-US
type: structure
uri: https://github.com/genealogy-standards/gedcom-evidence/v1/CONF

label: Confidence Level
specification: |
  Indicates confidence that evidence applies to a specific person.
  Scale: 0 (definitely not) to 5 (certain).
  Allows same evidence to be associated with multiple persons
  with different confidence levels.

payload: Integer

substructures: []

superstructures:
  - _EVID
```

## Validation Rules

1. **_EVID records**:
   - Must have at least one _ID
   - Must have at least one SOUR citation
   - _TYPE is optional but recommended

2. **_RDOC records**:
   - Must have _TEXT content
   - _CONC must include both _SUBJ and _TEXT
   - DATE is optional but recommended

3. **_RACT records**:
   - Must have _TYPE
   - Must have DATE
   - _RSLT should match activity outcome

4. **Evidence references**:
   - _CONF values must be 0-5
   - _RDOC should explain confidence level
   - Multiple persons can reference same _EVID

## Compatibility Notes

### Reading Systems
Systems that don't support this extension will:
- Ignore _EVID, _RDOC, _RACT records
- Ignore _EVID references on persons/families  
- Preserve all data for round-trip compatibility

### Writing Systems
When implementing this extension:
- Include extension URI in HEAD.SCHMA
- Maintain standard GEDCOM structures
- Use extension only for additional capability
- Provide migration tools for existing data

## Examples

See [use-cases.md](use-cases.md) for comprehensive examples.

## Version History

- v1.0 (2025-01-27): Initial specification based on 15 years of community input