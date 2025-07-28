# GEDCOM 7 Evidence Extension

## Overview

The GEDCOM Evidence Extension enables proper evidence-based genealogy by providing structures to separate what sources say (evidence) from what researchers conclude (conclusions). This extension is based on 15 years of community discussion, particularly insights from the BetterGEDCOM project (2010-2013).

## Why This Extension Exists

After analyzing 594 discussions with 4,970 replies from genealogy professionals and software developers, we identified a critical gap: **current genealogy software forces users to make premature conclusions about identity**. When you find a record mentioning "John Smith," you must immediately decide WHICH John Smith it refers to, potentially losing the original evidence if your conclusion proves wrong.

This extension solves that problem by allowing evidence to "float" until you determine through research which person it describes.

## Quick Start

```gedcom
# Evidence floating unattached
0 @E1@ _EVID
1 _ID John Smith
1 _ID son of Mary
1 _ID aged 40
1 SOUR @S1@
2 PAGE 1850 Census, p. 42

# Later, tentatively associate with persons
0 @I1@ INDI
1 NAME John /Smith/
1 _EVID @E1@
2 _CONF 3         # 60% confidence this is him

0 @I2@ INDI
1 NAME John /Smith/
1 _EVID @E1@
2 _CONF 2         # 40% confidence it's this one
```

## Core Concepts

### 1. Evidence Container (_EVID)
Stores what sources actually say, using identifiers rather than conclusions.

### 2. Research Documentation (_RDOC)
Documents analysis and reasoning, supporting GPS compliance.

### 3. Research Activity (_RACT)
Tracks research process, including negative searches.

## Documentation

- [Specification](specification.md) - Technical details and YAML definitions
- [Design Rationale](design-rationale.md) - Why we made these choices
- [Community Insights](community-insights.md) - 15 years of discussions analyzed
- [Use Cases](use-cases.md) - Real-world examples
- [Glossary](glossary.md) - Terms and concepts
- [Migration Guide](migration-guide.md) - Adopting the extension

## Key Benefits

1. **Preserves Original Evidence** - Never lose what sources actually said
2. **Supports Uncertain Identity** - Evidence can belong to multiple people
3. **Documents Research Process** - GPS compliance built-in
4. **Reversible Conclusions** - Change your mind without losing data
5. **Professional Standards** - Supports Evidence Explained methodology

## Compatibility

This extension is designed to degrade gracefully. Systems that don't support it will simply ignore the extension tags, preserving standard GEDCOM functionality.

## Community Foundation

This extension synthesizes insights from:
- 318 BetterGEDCOM wiki pages
- 594 community discussions
- 47 detailed proposals
- 74 contributing genealogists and developers

## Contributing

This extension is open for community input. Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

Released under the same terms as GEDCOM 7 specifications.