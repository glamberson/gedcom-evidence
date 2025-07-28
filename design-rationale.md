# Design Rationale: Why Evidence Must Float

## The Core Problem

### Current Software Forces Premature Conclusions

In every genealogy program today, when you find a source mentioning "John Smith, aged 40, son of Mary," you must immediately attach it to a specific John Smith in your database. But what if you have three John Smiths? What if you haven't created any John Smith yet? What if you later discover you attached it to the wrong one?

### Community Voice (2011)
> "When adding information from a source, current software has no separate record for what you get from evidence. Instead, this information is stored mixed with information from other origins, under one umbrella person. If your conclusions are wrong, it's not easy to undo them. You won't get back the evidence information, because it has been destroyed." - BetterGEDCOM Contributor

## The Solution: Floating Evidence with Identifiers

### Why Identifiers, Not Full Personas

Early proposals suggested creating complete "persona" records for each piece of evidence. This proved too complex. Our solution uses simple identifiers:

```gedcom
0 @E1@ _EVID
1 _ID John Smith          # Primary identifier from source
1 _ID son of Mary         # Relationship identifier  
1 _ID aged 40            # Descriptive identifier
1 SOUR @S1@
```

### Community Insight (2010)
> "The fundamental issue is having evidence that identifies 'John Smith' but not knowing WHICH John Smith. Is it the one who died in 1845 in Scranton? This evidence needs to exist floating in a research archive with the identifier 'John Smith' until associated with a person - or even two persons with uncertainty values." - Project Lead

## Key Design Decisions

### 1. Evidence as Independent Records

**Decision**: Make _EVID top-level records, not sub-structures.

**Rationale**: Evidence often relates to multiple people/families. It can't "belong" to just one person.

**Community Support** (171 replies):
> "Evidence records must be immutable leaves in the tree structure. Conclusion records group evidence records. Each grouping decision is documented. The process is fully reversible." - Senior Contributor

### 2. Multiple Associations with Confidence

**Decision**: Allow the same evidence to be claimed by multiple persons with different confidence levels.

**Rationale**: Mirrors real research where you're uncertain which person evidence describes.

```gedcom
# Same evidence, two possible people
0 @I1@ INDI
1 _EVID @E1@
2 _CONF 3    # 60% sure

0 @I2@ INDI  
1 _EVID @E1@
2 _CONF 2    # 40% sure
```

### 3. Simple Identifiers Over Complex Structures

**Decision**: Use flat _ID tags rather than nested structures.

**Original Proposal** (too complex):
```gedcom
1 _INFO John Smith
2 _ROLE Name
1 _INFO son of Mary  
2 _ROLE Relationship
```

**Simplified Design**:
```gedcom
1 _ID John Smith
1 _ID son of Mary
```

**Rationale**: Easier to implement, understand, and process.

### 4. Research Documentation Separate from Analysis

**Decision**: _RDOC for proof arguments, _RACT for research activities.

**Community Debate** (45 replies on negative evidence):
> "Some record what they've proven. Others record their research process, including failed searches. We need both." - Power User

### 5. GPS Compliance Built-In

**Decision**: Structure supports all five elements of the Genealogical Proof Standard.

**Elements Supported**:
1. Exhaustive search → _RACT tracks all searches
2. Complete citation → SOUR required on _EVID
3. Analysis/correlation → _RDOC documents reasoning
4. Conflict resolution → Multiple _EVID with _CONF
5. Sound conclusion → _RDOC with proof argument

## Addressing Historic Objections

### "This Creates Unproven Identities"

**Response**: We're not creating fake people. We're documenting what sources actually say. The identifiers help determine which real person the evidence describes.

### "Too Complex for Casual Users"

**Response**: It's optional. Basic GEDCOM works unchanged. This adds capability for serious researchers without forcing anyone to use it.

### "Programs Handle This Differently"

**Response**: Exactly why we need a standard. Our model can represent any program's approach to evidence.

## Learning from Failed Attempts

### GenTech Model (1990s)
- Too abstract with "assertions"
- Didn't match user mental models
- Our approach uses concrete "evidence" and "identifiers"

### GEDCOM X PersonView (2012)
- Right idea, complex implementation
- Limited to FamilySearch ecosystem
- We use simpler, more portable approach

### BetterGEDCOM (2010-2013)
- Never reached implementation
- Too many competing visions
- We synthesized the best ideas into practical design

## The Mental Model

### How Genealogists Think
1. Find evidence mentioning someone
2. Collect more evidence with similar identifiers
3. Analyze whether evidence refers to same person
4. Document reasoning for conclusions
5. Remain open to revising conclusions

### How Our Extension Supports This
1. _EVID records store evidence with identifiers
2. Multiple _EVID can have overlapping identifiers
3. _RDOC documents correlation analysis
4. _CONF values show certainty levels
5. Links can be changed without losing evidence

## Future-Proofing

### Extensibility Points
- Additional identifier types can be added
- Confidence models can be refined
- Research activities can be expanded
- Analysis structures can be enhanced

### Compatibility Strategy
- Unknown tags ignored by non-supporting systems
- Core GEDCOM structures unchanged
- Progressive enhancement model
- Clear migration paths

## Conclusion

This design represents 15 years of community wisdom distilled into practical structures. It solves real problems genealogists face daily while remaining simple enough to implement and understand. By allowing evidence to float with identifiers, we finally enable software to support how genealogical research actually works.