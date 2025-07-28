# Evidence Extension Use Cases

## Table of Contents
1. [Basic Evidence Recording](#basic-evidence-recording)
2. [Multiple Candidate Persons](#multiple-candidate-persons)
3. [Research Process Documentation](#research-process-documentation)
4. [Conflicting Evidence](#conflicting-evidence)
5. [Negative Evidence](#negative-evidence)
6. [Evidence Correlation](#evidence-correlation)
7. [Professional Workflows](#professional-workflows)

## Basic Evidence Recording

### Use Case 1: Census Record with Multiple People

**Scenario**: You find an 1850 census record listing a household.

**Traditional Approach** (forced conclusions):
```gedcom
0 @I1@ INDI
1 NAME John /Smith/
1 BIRT
2 DATE ABT 1810
2 SOUR @S1@
3 PAGE Hamilton County, Page 247, Line 12
```

**Evidence Extension Approach** (floating evidence):
```gedcom
# Evidence extracted from census
0 @E1@ _EVID
1 _ID John Smith
1 _ID head of household
1 _ID aged 40
1 _ID born in Ohio
1 SOUR @S1@
2 PAGE Hamilton County, Page 247, Line 12

0 @E2@ _EVID
1 _ID Mary Smith
1 _ID wife
1 _ID aged 35
1 _ID born in Pennsylvania
1 SOUR @S1@
2 PAGE Hamilton County, Page 247, Line 13

# Later, associate with persons
0 @I1@ INDI
1 NAME John /Smith/
1 BIRT
2 DATE ABT 1810
2 PLAC Ohio, USA
1 _EVID @E1@
2 _CONF 5
2 NOTE Matches age and location
```

**Benefits**:
- Original census information preserved exactly
- Can reassign evidence if wrong person identified
- Other researchers can evaluate your conclusion

## Multiple Candidate Persons

### Use Case 2: The "Which John Smith?" Problem

**Scenario**: Death certificate for "John Smith, son of Mary Smith" but you have three John Smiths in your database.

```gedcom
# Floating evidence
0 @E10@ _EVID
1 _ID John Smith
1 _ID son of Mary Smith
1 _ID died 15 Mar 1885
1 _ID aged 75 years
1 _ID birthplace Ohio
1 SOUR @S10@
2 PAGE Death Certificate #1885-234

# Three possible John Smiths
0 @I10@ INDI
1 NAME John /Smith/
1 _EVID @E10@
2 _CONF 2
2 _RDOC @R1@
2 NOTE Born ~1810, mother unknown

0 @I11@ INDI
1 NAME John /Smith/
1 _EVID @E10@
2 _CONF 4
2 _RDOC @R1@
2 NOTE Born ~1810, mother Mary Jones

0 @I12@ INDI
1 NAME John /Smith/
1 _EVID @E10@
2 _CONF 1
2 _RDOC @R1@
2 NOTE Born ~1812, mother Mary Smith

# Research documentation
0 @R1@ _RDOC
1 _TITL Identifying John Smith death certificate
1 _TEXT Three John Smiths are candidates for this death certificate.
2 CONT @I11@ is most likely (CONF 4) because his mother was Mary Jones
2 CONT who remarried a Smith. @I12@ has a mother Mary Smith but birth
2 CONT year is off by 2 years. @I10@ has unknown mother.
1 _EVID @E10@
1 AUTH Jane Genealogist
1 DATE 15 JAN 2025
```

## Research Process Documentation

### Use Case 3: Systematic Research Plan

**Scenario**: Searching for birth record of ancestor.

```gedcom
# Research plan
0 @RP1@ _RACT
1 _TYPE Search
1 _PURP Find birth record for John Smith @I1@
1 _TARG Birth record 1805-1815
1 _VENU County Courthouse
1 _RSLT NotFound
1 DATE 10 JAN 2025
1 NOTE Searched birth registers 1805-1815, no John Smith found

0 @RP2@ _RACT
1 _TYPE Search
1 _PURP Find birth record for John Smith @I1@
1 _TARG Church baptism records
1 _VENU St. Mary's Church records on FamilySearch
1 _RSLT Found
1 _FIND John Smith baptized 12 May 1810
2 _EVID @E20@
1 DATE 11 JAN 2025

# Evidence from successful search
0 @E20@ _EVID
1 _ID John Smith
1 _ID son of William and Mary
1 _ID baptized 12 May 1810
1 _ID born 10 May 1810
1 SOUR @S20@
2 PAGE St. Mary's Baptism Register, Page 45
```

## Conflicting Evidence

### Use Case 4: Different Birth Dates

**Scenario**: Multiple sources give different birth dates.

```gedcom
# Evidence 1: Census says age 40 in 1850
0 @E30@ _EVID
1 _ID John Smith
1 _ID aged 40
1 _ID enumerated June 1850
1 SOUR @S30@
2 PAGE 1850 Census

# Evidence 2: Death certificate says age 76 in 1885
0 @E31@ _EVID
1 _ID John Smith  
1 _ID aged 76 years
1 _ID died March 1885
1 SOUR @S31@
2 PAGE Death Certificate

# Evidence 3: Baptism record
0 @E32@ _EVID
1 _ID John Smith
1 _ID baptized 12 May 1810
1 _ID born 10 May 1810
1 SOUR @S32@
2 PAGE Church Register

# Analysis of conflicts
0 @R10@ _RDOC
1 _TITL Birth year analysis for John Smith
1 _TEXT Three sources suggest different birth years:
2 CONT - Census: age 40 in 1850 = born ~1810
2 CONT - Death: age 76 in 1885 = born ~1809  
2 CONT - Baptism: born 10 May 1810
2 CONT Baptism record is primary evidence, most reliable.
2 CONT Census ages often rounded. Death certificate informant
2 CONT may have estimated age.
1 _EVID @E30@
1 _EVID @E31@
1 _EVID @E32@
1 _CONC
2 _SUBJ @I1@
2 _TEXT Birth date established as 10 May 1810 based on baptism record

# Person with all evidence
0 @I1@ INDI
1 NAME John /Smith/
1 BIRT
2 DATE 10 MAY 1810
1 _EVID @E30@
2 _CONF 3
1 _EVID @E31@
2 _CONF 3
1 _EVID @E32@
2 _CONF 5
2 _RDOC @R10@
```

## Negative Evidence

### Use Case 5: Recording What Wasn't Found

**Scenario**: Thorough search finds no evidence of claimed military service.

```gedcom
# Research activity - negative result
0 @RA1@ _RACT
1 _TYPE Search
1 _PURP Verify Civil War service for John Smith @I1@
1 _TARG Military service records
1 _VENU National Archives, Fold3, local records
1 _RSLT NotFound
1 DATE 20 JAN 2025
1 NOTE Searched:
2 CONT - Civil War pension files
2 CONT - Compiled Military Service Records
2 CONT - Regimental histories for claimed unit
2 CONT - Local enrollment lists
2 CONT No evidence of service found

# Documentation
0 @R20@ _RDOC
1 _TITL Civil War service claim analysis
1 _TEXT Family tradition claims John Smith served in Civil War.
2 CONT Exhaustive search found no evidence. He would have been
2 CONT age 51-55 during war, possibly too old. No pension applied
2 CONT for by him or widow. Conclusion: Did not serve.
1 _SUBJ @I1@
1 AUTH Jane Genealogist
1 DATE 20 JAN 2025

# Link negative evidence to person
0 @I1@ INDI
1 NAME John /Smith/
1 NOTE No military service found despite family tradition
1 _RDOC @R20@
```

## Evidence Correlation

### Use Case 6: Building Identity Through Multiple Sources

**Scenario**: Correlating evidence to prove identity.

```gedcom
# Evidence from different sources
0 @E40@ _EVID
1 _ID John Smith
1 _ID married Jane Doe
1 _ID 15 Jun 1835
1 _ID Hamilton County, Ohio
1 SOUR @S40@

0 @E41@ _EVID
1 _ID John Smith
1 _ID son of William Smith
1 _ID born ~1810
1 SOUR @S41@

0 @E42@ _EVID
1 _ID John Smith
1 _ID bought land from William Smith
1 _ID 10 Dec 1834
1 _ID consideration $1 "love and affection"
1 SOUR @S42@

# Correlation analysis
0 @R30@ _RDOC
1 _TITL Proving identity of John Smith
1 _TEXT Evidence correlation:
2 CONT 1. John Smith married Jane Doe 1835 in Hamilton County
2 CONT 2. John Smith son of William per family Bible
2 CONT 3. John Smith bought land from William Smith for $1
2 CONT The $1 "love and affection" indicates family relationship.
2 CONT Timeline fits: land purchase Dec 1834, marriage Jun 1835.
2 CONT Conclusion: Same John Smith in all three records.
1 _EVID @E40@
1 _EVID @E41@
1 _EVID @E42@
1 _CONC
2 _SUBJ @I1@
2 _TEXT All three evidence records refer to same person
1 DATE 25 JAN 2025
```

## Professional Workflows

### Use Case 7: Client Research Report

**Scenario**: Professional genealogist documenting research for client.

```gedcom
# GPS-compliant research
0 @RP10@ _RACT
1 _TYPE Analysis
1 _PURP Identify parents of Mary Johnson @I50@
1 _PLAN Research Plan:
2 CONT 1. Search vital records
2 CONT 2. Check census records
2 CONT 3. Review probate files
2 CONT 4. Examine land records
1 _STATUS Completed
1 DATE 30 JAN 2025

# Multiple evidence pieces found
0 @E50@ _EVID
1 _ID Mary Johnson
1 _ID daughter of Robert Johnson
1 _ID named in will
1 SOUR @S50@

0 @E51@ _EVID
1 _ID Mary age 10
1 _ID in household of Robert Johnson
1 SOUR @S51@

# GPS-compliant proof argument
0 @R40@ _RDOC
1 _TITL Parentage of Mary Johnson
1 _TEXT GPS-compliant proof argument:
2 CONT 1. EXHAUSTIVE SEARCH: Searched vital, census, probate, land
2 CONT 2. COMPLETE CITATIONS: All sources fully documented
2 CONT 3. ANALYSIS: Will names "daughter Mary," census shows Mary age 10
2 CONT    in Robert's household 1850, age matches birth year
2 CONT 4. CONFLICTS: None found
2 CONT 5. CONCLUSION: Robert Johnson is father of Mary Johnson
1 _EVID @E50@
1 _EVID @E51@
1 _GPS Compliant
1 AUTH Professional Genealogist, CG
1 DATE 30 JAN 2025

# Link to person
0 @I50@ INDI
1 NAME Mary /Johnson/
1 FAMC @F10@
1 _EVID @E50@
2 _CONF 5
1 _EVID @E51@
2 _CONF 5
1 _RDOC @R40@

0 @F10@ FAM
1 HUSB @I51@
1 CHIL @I50@

0 @I51@ INDI
1 NAME Robert /Johnson/
```

## Summary

These use cases demonstrate how the Evidence Extension:

1. **Preserves** exactly what sources say
2. **Enables** uncertain associations
3. **Documents** research reasoning
4. **Supports** professional standards
5. **Handles** complex real-world scenarios
6. **Tracks** both positive and negative evidence
7. **Facilitates** GPS compliance

The extension provides the tools genealogists need to practice evidence-based genealogy while maintaining compatibility with existing GEDCOM structures.