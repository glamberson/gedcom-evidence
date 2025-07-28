# Evidence Extension Glossary

## Core Concepts

### Evidence
Information extracted from a source that provides genealogical data. Evidence exists independently of conclusions about which person it describes.

### Evidence Container (_EVID)
A GEDCOM record that holds evidence extracted from sources. Uses identifiers to describe who or what the evidence refers to without making conclusions about specific persons.

### Floating Evidence
Evidence that exists independently in the system, not yet associated with any specific person or family. Can be linked to multiple candidates with different confidence levels.

### Identifier (_ID)
A text fragment from a source that helps identify the subject of evidence. Examples: "John Smith", "son of Mary", "aged 40", "blacksmith".

### Conclusion
A decision about which person(s) a piece of evidence describes, based on analysis and correlation with other evidence.

### Confidence (_CONF)
A numerical value (0-5) indicating certainty about an association between evidence and a person. 0 = no confidence, 5 = certain.

## Record Types

### _EVID (Evidence Container)
Top-level record storing evidence with identifiers and required source citation.

### _RDOC (Research Documentation)  
Top-level record documenting analysis, reasoning, and proof arguments about evidence and conclusions.

### _RACT (Research Activity)
Top-level record tracking research actions, including searches, reviews, and their results.

## Structure Elements

### _ID (Identifier)
Sub-element of _EVID containing text that identifies the subject from the source.

### _TYPE (Type)
Classification element used in _EVID (Person|Event|Relationship|Place|Other) and _RACT (Search|Review|Analysis|Correspondence).

### _PURP (Purpose)
The goal or objective of a research activity.

### _TARG (Target)
What was being sought in a research activity.

### _VENU (Venue)
Where research was conducted (repository, website, location).

### _RSLT (Result)
Outcome of research activity (Found|NotFound|Partial|Pending).

### _FIND (Finding)
What was discovered during a research activity.

### _TITL (Title)
Title of a research documentation.

### _TEXT (Text)
Main content of research documentation or analysis.

### _SUBJ (Subject)
Reference to what is being analyzed or discussed.

### _CONC (Conclusion)
Conclusions reached in research documentation.

### _RSRCH (Researcher)
Name of person conducting research.

### _STATUS (Status)
Current state of a research plan or activity.

### _GPS (GPS Compliance)
Indicates Genealogical Proof Standard compliance.

## Research Concepts

### Evidence-Based Genealogy
Methodology that separates evidence (what sources say) from conclusions (what we determine about persons).

### Genealogical Proof Standard (GPS)
Five-element standard for genealogical proof:
1. Exhaustive search
2. Complete source citations
3. Analysis and correlation
4. Resolution of conflicts
5. Sound written conclusion

### Direct Evidence
Evidence that directly states a conclusion without need for interpretation. Example: Birth certificate stating date of birth.

### Indirect Evidence
Evidence requiring interpretation or correlation with other evidence. Example: Age in census suggesting birth year.

### Negative Evidence
The absence of expected information. Example: No birth record found despite thorough search.

### Conflicting Evidence
Evidence that contradicts other evidence. Must be resolved through analysis.

### Proof Argument
Written explanation of how evidence leads to a conclusion, addressing any conflicts.

### Evidence Correlation
Process of comparing multiple pieces of evidence to determine if they refer to the same person/event.

### Identity Resolution
Determining which person in a database corresponds to a person mentioned in evidence.

## Confidence Levels

### 0 - No Confidence
Evidence definitely does not apply to this person.

### 1 - Very Low
Evidence might apply but probably doesn't (20% likely).

### 2 - Low  
Evidence possibly applies (40% likely).

### 3 - Medium
Evidence probably applies (60% likely).

### 4 - High
Evidence very likely applies (80% likely).

### 5 - Certain
Evidence definitely applies to this person (100% certain).

## Workflow Terms

### Premature Conclusion
Assigning evidence to a specific person before thorough analysis, risking incorrect associations.

### Research Log
Record of research activities including what was searched, where, when, and results.

### Round-trip Fidelity
Ability to export and re-import data without loss of information or meaning.

### Progressive Enhancement
Design approach where advanced features enhance but don't break basic functionality.

### Backward Compatibility
New features don't prevent older software from reading basic GEDCOM data.

## Implementation Terms

### Immutable Evidence
Evidence records never change after creation. New interpretations create new records.

### Evidence Tree
Conceptual structure where evidence records are leaves and conclusion records are branches.

### Association
Link between evidence and a person/family with confidence level.

### Degrading Gracefully
Unknown extension tags are ignored by non-supporting software without errors.

## Community Terms

### PFACT
Term from BetterGEDCOM meaning "piece of fact" - atomic genealogical information.

### Persona
Earlier term for evidence-based person record. Replaced by simpler "evidence container" concept.

### Administrative Person
GenTech term for person record created for technical reasons, not representing real person.

### Conclusion Person
Traditional GEDCOM person record containing concluded information.

### Evidence Person
Earlier term for evidence container. Avoided to prevent confusion with regular persons.

## Usage Notes

- Terms in CAPS (like _EVID) are GEDCOM tags
- Terms in italics are conceptual
- Numbers in parentheses (0-5) indicate valid ranges
- Pipe symbols (|) separate valid options