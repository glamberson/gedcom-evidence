# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.4] - 2025-07-29

### Fixed
- Corrected YAML indentation in enumeration files as requested by GEDCOM registry maintainer
- All list items in YAML files now properly indented with 2 spaces

### Changed
- Updated enumeration files: enum-High.yaml, enum-Medium.yaml, enum-Low.yaml, enum-Hypothesis.yaml

## [0.1.1] - 2025-01-28

### Added
- Registry-compliant YAML files in `registry-yaml/` directory
- VERSION file
- CHANGELOG.md file
- Link to GEDCOM Registry PR #173 in README

### Changed
- Updated README to reference registry submission status

### Technical Details
- Added 12 YAML files validated by official GEDCOM registry validator:
  - 7 structure definitions (_EVID, _ID, _FIND, _RDOC, _RACT, _CONC, _CONF)
  - 4 enumeration definitions (High, Medium, Low, Hypothesis)
  - 1 enumeration-set definition (Confidence)

## [0.1.0] - 2025-01-27

### Added
- Initial release
- Evidence container records (_EVID)
- Research findings (_FIND, _RDOC, _RACT)
- Identity tracking (_ID)
- Confidence assessments (_CONC, _CONF)
- Basic examples
- Comprehensive documentation