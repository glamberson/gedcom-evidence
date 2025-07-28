# Community Insights: 15 Years of Evidence/Conclusion Discussions

## Overview

This document captures key insights from analyzing 594 discussions with 4,970 replies about evidence and conclusion modeling in genealogy software. Names have been anonymized, but the insights remain unchanged.

## The Fundamental Problem (2010-2025)

### The "Which John Smith?" Dilemma

**Community Member A** (2010):
> "A fundamental issue in the research process is having a piece of evidence that says 'John Smith was born.' But what John Smith is it? Is it the John Smith who died in 1845 in Scranton, PA? No one is sure what this great evidence identifies until it is associated with a person (or even 2 with an uncertainty value for each association)."

**Senior Contributor B** (2011):
> "Current software forces you to throw away the PFACT that you think less likely. If, later, you see that you made a wrong decision, then you'll have to re-enter the old PFACT including its source - and probably throw away the other PFACT again!"

### The Forced Conclusion Problem

**Power User C** (2011):
> "You find a record mentioning facts about a person. To your surprise, some details conflict with what you already 'knew.' Now you must decide what to do. But it's probably not going to be easy to 'source' one part separately from the other. If you don't make notes to keep track, you might forget where which part came from."

## Key Debates and Resolutions

### 1. Evidence Persons vs. Conclusion Persons (171 replies)

**Original Proposal by Contributor D** (2010):
> "Evidence records are immutable leaves. Conclusion records group evidence records. Each grouping decision is documented. The process is fully reversible."

**Objection by Researcher E** (2010):
> "I am not a fan of automated family histories. This seems like a rationale for creating, publishing, archiving and exchanging unproven identities."

**Resolution by Contributor D**:
> "I see a citation as a formatted text string that describes where evidence can be found. An evidence record is the container for the information extracted from that source. They serve different purposes."

### 2. The Mental Model Debate (63 replies)

**Developer F** (2011):
> "The evidence/conclusion process is an exact analogue of what genealogists do in their heads or on paper. Using evidence and conclusion records simply models this process."

**Skeptic G's Challenge**:
> "I fail to see why we need all these extra steps to record the information."

**Community Response by User H**:
> "Some people want to record all their thoughts/hypotheses in their program, even if not yet proven. Others only put things into the database that they've proven already. A good program needs to accommodate both kinds of users."

### 3. Negative Evidence (45 replies)

**Researcher I** (2012):
> "Searched courthouse birth records 1805-1815, found nothing. This negative evidence is as important as positive findings. Where do I record this?"

**Solution Advocate J**:
> "Research activities must track what was sought, where, when, and the result - including 'NotFound.' This is GPS compliance."

### 4. Identity and Proof (40 replies)

**Analyst K** (2011):
> "Direct evidence that is uncontroverted doesn't require a proof, right?"

**Professional L's Response**:
> "What we need somewhere attached is the 'proof of identity' - i.e., is it 'my' Simeon Doe? Something like 'parents William and Mary' or 'married Jane Brown.'"

## Implementation Challenges

### The Complexity Objection

**Vendor Representative M** (2012):
> "Even if the receiving program was sent an ideal set of data, the result will not be the same as in the sending program. Different programs have 170, 400, or 1500 templates."

**Standards Advocate N**:
> "Then why not define the standard set? Those programs will then have THE standard to use."

### The Backward Compatibility Requirement

**GEDCOM Expert O** (2011):
> "Whatever we do must degrade gracefully for non-supporting software. Unknown tags should be ignored, preserving standard functionality."

## Synthesis: What the Community Needs

### Core Requirements (Distilled from 594 discussions)

1. **Floating Evidence** (218 mentions)
   - Evidence must exist independently
   - Identifiers connect evidence to persons
   - Multiple associations with confidence levels

2. **Immutable Evidence Records** (156 mentions)
   - Evidence represents what sources actually say
   - Never modified after creation
   - New interpretations create new records

3. **Documented Reasoning** (134 mentions)
   - GPS compliance requires proof arguments
   - Correlation decisions must be recorded
   - Process must be reversible

4. **Research Process Support** (89 mentions)
   - Negative searches must be recordable
   - Research plans and objectives
   - Task tracking and completion

5. **Identity Resolution** (76 mentions)
   - Support uncertain associations
   - Allow competing hypotheses
   - Document identity reasoning

## Consensus Principles

After 15 years of debate, these principles emerged:

### 1. Separation is Essential
> "Software that can't separate evidence from conclusions forces data loss and poor research practices." - Multiple contributors

### 2. Flexibility is Required
> "Don't force one methodology. Enable professional standards while supporting casual users." - Community consensus

### 3. Simplicity Wins
> "Complex models fail adoption. Simple, understandable structures succeed." - Implementation experience

### 4. Standards Enable Progress
> "Without standards, every transfer loses data. With standards, genealogy can advance." - Vendor perspective

## Lessons for Implementation

### What Failed

1. **Over-abstraction** (GenTech assertions)
2. **Forced methodology** (One true way)
3. **Complex hierarchies** (Nested personas)
4. **Vendor lock-in** (Proprietary approaches)

### What Succeeded

1. **Concrete concepts** (Evidence, not assertions)
2. **Optional adoption** (Enhance, don't replace)
3. **Simple structures** (Flat, not nested)
4. **Open standards** (Community-driven)

## The Path Forward

The community has spoken clearly through thousands of messages:

1. **We need floating evidence with identifiers**
2. **We need to document our reasoning**
3. **We need to track our research process**
4. **We need standards that work**

This extension embodies these requirements in the simplest possible form that still solves the real problems genealogists face every day.

## Acknowledgments

This synthesis draws from contributions by 74 BetterGEDCOM participants and hundreds of other genealogists who shared their experiences and insights over 15 years. Their collective wisdom made this design possible.