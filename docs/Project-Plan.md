# myWriteOffs-data Project Plan

## Overview

This repository hosts the **public valuation data** for the myWriteOffs application. It provides a version-controlled, transparent source of donation item valuations compiled from public IRS-recommended sources (Goodwill, Salvation Army, IRS Publication 561).

The data is structured to:

- Support the app's 3-layer data model (Base → Cache → User)
- Enable periodic updates without requiring app releases
- Preserve user customizations during data updates
- Maintain legal compliance and patent safety through public source attribution

## Repository Purpose

- **Primary Function**: Host JSON data files containing item categories, valuations, and metadata
- **Update Frequency**: Quarterly (January, April, July, October) as a best effort attempt to keep data current and relevant for tax purposes
- **Access Pattern**: Public read-only via GitHub raw URLs
- **Version Control**: Git tags for each data release (v1.0.0, v1.1.0, etc.)
- **Legal Compliance**: All data sourced from public guides with full attribution

## Project Structure

```
myWriteOffs-data/
├── README.md                    # Repository overview and usage
├── CHANGELOG.md                 # Version history and changes
├── LICENSE                      # Data license (CC0 or similar)
├── docs/
│   ├── Project-Plan.md         # This file
│   ├── data-sources.md         # Detailed source documentation
│   ├── scraping-guide.md       # How to update data
│   └── validation-rules.md     # Data quality standards
├── data/
│   ├── version.json            # Current version metadata
│   ├── categories/
│   │   ├── clothing.json
│   │   ├── household.json
│   │   ├── electronics.json
│   │   ├── furniture.json
│   │   ├── books-media.json
│   │   └── sports-recreation.json
│   ├── items/
│   │   ├── clothing/
│   │   │   ├── mens-suits.json
│   │   │   ├── mens-shirts.json
│   │   │   ├── womens-dresses.json
│   │   │   ├── womens-blouses.json
│   │   │   └── shoes.json
│   │   ├── household/
│   │   │   ├── kitchenware.json
│   │   │   ├── linens.json
│   │   │   └── small-appliances.json
│   │   ├── electronics/
│   │   │   ├── computers.json
│   │   │   └── audio-video.json
│   │   └── furniture/
│   │       ├── living-room.json
│   │       └── bedroom.json
│   └── metadata/
│       ├── sources.json        # Source attribution
│       ├── condition-terms.json # IRS-approved condition terms
│       └── valuation-methods.json
├── scripts/
│   ├── scrape-goodwill.py      # Goodwill data scraper
│   ├── parse-salvation-army.py # Salvation Army PDF parser
│   ├── validate-data.py        # Data validation script
│   ├── generate-version.py     # Version file generator
│   └── requirements.txt        # Python dependencies
├── archive/
│   ├── 2024-Q1/
│   ├── 2024-Q2/
│   └── ...
└── tests/
    ├── test_data_integrity.py
    └── test_json_schema.py
```

## Technical Constraints

- All scripts must be written in Python 3.8+ with minimal dependencies (called from requirements.txt)
- Scripts must include proper error handling and logging
- All data must be validated against JSON schemas before being committed
- Scripts must be idempotent and safe to run multiple times
- All scripts must be compatible with the CI/CD pipeline
- All scripts must be documented with docstrings and inline comments
- No other languages (node.js, typscript, etc) -- only Python allowed for all automation scripts
- All automation scripts must be self-contained and not require external build steps

## Data Schema

### version.json

```json
{
  "version": "1.0.0",
  "releaseDate": "2024-01-15T00:00:00Z",
  "changelog": "Initial release with Goodwill and Salvation Army valuations",
  "dataFiles": {
    "categories": "https://raw.githubusercontent.com/yourorg/myWriteOffs-data/main/data/categories/",
    "items": "https://raw.githubusercontent.com/yourorg/myWriteOffs-data/main/data/items/"
  },
  "sources": [
    {
      "id": "goodwill",
      "name": "Goodwill Valuation Guide",
      "lastUpdated": "2024-01-15",
      "url": "https://www.goodwill.org/donate/donation-value-guide/"
    },
    {
      "id": "salvation-army",
      "name": "Salvation Army Valuation Guide",
      "lastUpdated": "2024-01-15",
      "url": "https://satruck.org/Home/DonationValueGuide"
    },
    {
      "id": "irs-pub-561",
      "name": "IRS Publication 561",
      "lastUpdated": "2024-01-15",
      "url": "https://www.irs.gov/pub/irs-pdf/p561.pdf"
    }
  ],
  "minimumAppVersion": "1.0.0",
  "totalItems": 500,
  "totalCategories": 25
}
```

### Category File (e.g., clothing.json)

```json
{
  "id": "clothing",
  "name": "Clothing",
  "description": "Apparel and accessories for men, women, and children",
  "parent": null,
  "subcategories": [
    "mens-clothing",
    "womens-clothing",
    "childrens-clothing",
    "shoes",
    "accessories"
  ],
  "icon": "tshirt",
  "sortOrder": 1
}
```

### Item File (e.g., mens-suits.json)

```json
{
  "items": [
    {
      "id": "mens-suit-001",
      "name": "Men's Suit",
      "description": "Two-piece suit (jacket and pants)",
      "category": "mens-clothing",
      "tags": ["formal", "business", "professional"],
      "valuations": [
        {
          "sourceId": "goodwill",
          "value": 25.00,
          "condition": "good",
          "notes": "Clean, no stains or tears"
        },
        {
          "sourceId": "salvation-army",
          "value": 30.00,
          "condition": "good",
          "notes": "Good condition, minor wear acceptable"
        }
      ],
      "recommendedValue": 27.50,
      "valuationMethod": "Thrift Shop Value",
      "unit": "each",
      "lastUpdated": "2024-01-15T00:00:00Z"
    }
  ]
}
```

## Phase Breakdown

### Phase 1: Repository Setup & Infrastructure
**Goal:** Establish repository structure and basic tooling

- ✅ Create public GitHub repository `myWriteOffs-data`
- ✅ Initialize with README, LICENSE (CC-BY 4.0 for data)
- ✅ Set up directory structure (data/, scripts/, archive/, tests/)
- ✅ Create `.gitignore` for Python and temporary files
- ✅ Set up GitHub Actions for automated validation (PRs to main only)
- ✅ Create issue templates for data corrections
- 🔳 Set up branch protection rules (require PR reviews) - *Requires GitHub remote*
- ✅ Create initial CHANGELOG.md
- ✅ Document contribution guidelines

**Deliverables:**
- ✅ Functional repository with proper structure
- ✅ Minimal GitHub Actions workflow for cost-effective validation
- ✅ Clear documentation for contributors

**Status:** Phase 1 Complete (except branch protection which requires GitHub remote)

---

### Phase 2: Data Collection & Scraping Scripts
**Goal:** Build tools to collect valuation data from public sources

#### 2.1: Goodwill Scraper
- 🔳 Research Goodwill website structure and data format
- 🔳 Create Python scraper for Goodwill Valuation Guide
- 🔳 Parse HTML tables into structured data
- 🔳 Extract item names, categories, and valuations
- 🔳 Handle pagination and category navigation
- 🔳 Add error handling and retry logic
- 🔳 Save raw scraped data with timestamps

#### 2.2: Salvation Army Parser
- 🔳 Download Salvation Army Valuation Guide PDF
- 🔳 Create PDF parser using PyPDF2 or pdfplumber
- 🔳 Extract tables and text data
- 🔳 Parse item names and valuations
- 🔳 Handle multi-page tables
- 🔳 Normalize data format to match Goodwill structure
- 🔳 Save parsed data with source attribution

#### 2.3: IRS Publication 561 Parser
- 🔳 Download IRS Publication 561 PDF
- 🔳 Extract relevant guidelines and examples
- 🔳 Parse condition definitions and valuation methods
- 🔳 Create metadata files for condition terms
- 🔳 Document IRS-approved valuation approaches

#### 2.4: Data Normalization
- 🔳 Create unified data schema for all sources
- 🔳 Build normalization script to standardize formats
- 🔳 Map source-specific categories to unified taxonomy
- 🔳 Handle unit conversions (each, pair, set)
- 🔳 Calculate recommended values (average/median)
- 🔳 Add source attribution to each valuation

**Deliverables:**
- Working scrapers for all data sources
- Normalized JSON data files
- Source attribution metadata

---

### Phase 3: Data Validation & Quality Assurance
**Goal:** Ensure data integrity and consistency

#### 3.1: Local Validation Scripts
- 🔳 Create comprehensive Python validation scripts for local development
- 🔳 Create JSON schema definitions for all data types
- 🔳 Build validation script to check schema compliance
- 🔳 Validate required fields (id, name, valuations)
- 🔳 Check value ranges (no negative prices)
- 🔳 Verify category references are valid
- 🔳 Ensure source IDs match sources.json
- 🔳 Check for duplicate item IDs
- 🔳 Validate date formats and timestamps
- 🔳 Add command-line options for quick vs. full validation
- 🔳 Create pre-commit hook integration for immediate feedback

#### 3.2: Data Quality Checks
- 🔳 Flag items with missing valuations
- 🔳 Identify outliers (values >3 std deviations)
- 🔳 Check for inconsistent naming conventions
- 🔳 Verify all items have categories
- 🔳 Ensure descriptions are present and meaningful
- 🔳 Validate URL formats in sources
- 🔳 Check for broken category hierarchies

#### 3.3: GitHub Actions (Minimal, Cost-Effective)
- 🔳 Set up GitHub Actions to run only on PRs to main branch (not on pushes)
- 🔳 Configure minimal validation workflow (JSON schema only) to reduce costs
- 🔳 Add PR template with manual validation checklist
- 🔳 Create branch protection requiring PR review and validation passing
- 🔳 Document cost-conscious CI/CD approach in repository documentation

#### 3.4: Local Testing
- 🔳 Create unit tests for validation functions
- 🔳 Test edge cases (empty files, malformed JSON)
- 🔳 Test with sample data from each source
- 🔳 Create integration tests for full data pipeline
- 🔳 Add local test runner script for comprehensive validation

**Deliverables:**
- Comprehensive local validation suite
- Minimal GitHub Actions workflow (PRs to main only)
- Manual validation checklist and PR templates
- Data quality reports and documentation

---

### Phase 4: Initial Data Population
**Goal:** Create first production dataset (v1.0.0)

#### 4.1: Category Structure
- 🔳 Define top-level categories (10-15 major categories)
- 🔳 Create subcategory hierarchy (2-3 levels deep)
- 🔳 Assign icons/symbols to each category
- 🔳 Set sort order for logical browsing
- 🔳 Create category JSON files
- 🔳 Document category taxonomy

#### 4.2: Item Data Collection
- 🔳 Run Goodwill scraper and collect data
- 🔳 Run Salvation Army parser and collect data
- 🔳 Parse IRS Publication 561 examples
- 🔳 Normalize all collected data
- 🔳 Merge valuations from multiple sources
- 🔳 Calculate recommended values
- 🔳 Organize items into category folders
- 🔳 Target: 300-500 items for v1.0.0

#### 4.3: Metadata Files
- 🔳 Create sources.json with full attribution
- 🔳 Create condition-terms.json (IRS-approved terms)
- 🔳 Create valuation-methods.json
- 🔳 Document data collection dates
- 🔳 Add source URLs and access dates

#### 4.4: Version 1.0.0 Release
- 🔳 Run full validation suite
- 🔳 Generate version.json file
- 🔳 Create CHANGELOG entry
- 🔳 Archive source documents in archive/2024-Q1/
- 🔳 Tag release as v1.0.0
- 🔳 Create GitHub release with notes
- 🔳 Update README with usage examples

**Deliverables:**
- Production-ready v1.0.0 dataset
- 300-500 items across major categories
- Full source attribution and documentation

---

### Phase 5: App Integration Support
**Goal:** Ensure data format works seamlessly with app

#### 5.1: Data Access Testing
- 🔳 Test GitHub raw URL access for all files
- 🔳 Verify CORS headers allow app access
- 🔳 Test version.json download and parsing
- 🔳 Test category file downloads
- 🔳 Test item file downloads
- 🔳 Measure download sizes and performance
- 🔳 Test with slow network conditions

#### 5.2: Sample Integration Code
- 🔳 Create example Swift code for downloading data
- 🔳 Create example parsing code
- 🔳 Document expected data structures
- 🔳 Provide sample merge logic
- 🔳 Add to README for app developers

#### 5.3: API Documentation
- 🔳 Document all JSON schemas
- 🔳 Create API reference for data access
- 🔳 Document versioning strategy
- 🔳 Explain update/merge process
- 🔳 Provide migration guides for schema changes

**Deliverables:**
- Verified data access from app
- Integration documentation
- Sample code and examples

---

### Phase 6: Maintenance & Update Workflow
**Goal:** Establish sustainable update process

#### 6.1: Update Procedures
- 🔳 Document quarterly update schedule
- 🔳 Create checklist for data updates
- 🔳 Define version numbering scheme (semantic versioning)
- 🔳 Create update workflow diagram
- 🔳 Document rollback procedures
- 🔳 Create data diff tools to show changes

#### 6.2: Automation
- 🔳 Create script to run all scrapers in sequence
- 🔳 Automate data normalization pipeline
- 🔳 Automate validation checks
- 🔳 Auto-generate version.json from data
- 🔳 Create release preparation script
- 🔳 Set up scheduled GitHub Actions for scraping

#### 6.3: Monitoring
- 🔳 Set up alerts for scraper failures
- 🔳 Monitor source website changes
- 🔳 Track data quality metrics over time
- 🔳 Log download statistics (if possible)
- 🔳 Monitor GitHub issues for data corrections

**Deliverables:**
- Documented update workflow
- Automated update pipeline
- Monitoring and alerting

---

### Phase 7: Community & Governance
**Goal:** Enable community contributions while maintaining quality

#### 7.1: Contribution Guidelines
- 🔳 Create CONTRIBUTING.md
- 🔳 Define data submission standards
- 🔳 Create PR template for data updates
- 🔳 Document review process
- 🔳 Set up code owners for data files
- 🔳 Create issue templates for corrections

#### 7.2: Community Features
- 🔳 Enable GitHub Discussions for questions
- 🔳 Create FAQ document
- 🔳 Set up wiki for extended documentation
- 🔳 Create examples of common contributions
- 🔳 Recognize contributors in CHANGELOG

#### 7.3: Legal & Compliance
- 🔳 Add legal disclaimer to README
- 🔳 Document data provenance for all items
- 🔳 Maintain copies of source documents
- 🔳 Create audit trail documentation
- 🔳 Review and update license as needed
- 🔳 Ensure no proprietary Intuit data included

**Deliverables:**
- Open contribution process
- Legal compliance documentation
- Active community engagement

---

## Quarterly Maintenance Process

### Pre-Update (Week 1)
1. Check source websites for updates
2. Download latest source documents
3. Archive current data version
4. Create new branch for update

### Data Collection (Week 2)
1. Run all scraping scripts
2. Parse new data
3. Compare with previous version
4. Flag significant changes (>20% variance)
5. Manual review of flagged items

### Validation & Testing (Week 3)
1. Run validation suite
2. Check data quality metrics
3. Test sample downloads
4. Review community-reported issues
5. Make corrections as needed

### Release (Week 4)
1. Update version.json
2. Update CHANGELOG.md
3. Create GitHub release
4. Tag with version number
5. Announce update to community
6. Monitor for issues

## Success Metrics

### Data Quality
- **Coverage**: 500+ items across 20+ categories
- **Accuracy**: <5% error rate in valuations
- **Freshness**: Updated within 30 days of source changes
- **Completeness**: >95% of items have all required fields

### Repository Health
- **Validation**: 100% of data passes validation
- **Documentation**: All schemas documented
- **Tests**: >90% code coverage in scripts
- **Issues**: <7 day average response time

### App Integration
- **Download Success**: >99% success rate
- **Performance**: <5 seconds for full data download
- **Compatibility**: Works with app versions 1.0+

## Risk Management

### Technical Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| Source website changes | High | Monitor sites, maintain scrapers, manual fallback |
| Data corruption | High | Validation scripts, version control, backups |
| GitHub downtime | Medium | App caches data locally, graceful degradation |
| Breaking schema changes | High | Semantic versioning, migration guides |

### Legal Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| Copyright claims | High | Only use public sources, full attribution |
| Patent infringement | High | Avoid Intuit-specific methods, document differences |
| Data accuracy liability | Medium | Disclaimer, cite sources, community review |

### Operational Risks
| Risk | Impact | Mitigation |
|------|--------|------------|
| Maintainer availability | Medium | Document all processes, automate where possible |
| Community spam/vandalism | Low | PR reviews, branch protection, code owners |
| Scraper failures | Medium | Error handling, alerts, manual backup process |

## Timeline

### Immediate (Weeks 1-2)
- Phase 1: Repository setup
- Phase 2.1: Goodwill scraper

### Short-term (Weeks 3-6)
- Phase 2.2-2.4: Complete data collection
- Phase 3: Validation and testing
- Phase 4.1-4.2: Initial data population

### Medium-term (Weeks 7-10)
- Phase 4.3-4.4: v1.0.0 release
- Phase 5: App integration support
- Phase 6.1: Update procedures

### Long-term (Weeks 11+)
- Phase 6.2-6.3: Automation and monitoring
- Phase 7: Community features
- Ongoing: Quarterly updates

## Next Steps

**Before starting development, please review and confirm:**

1. **Repository naming**: Is `myWriteOffs-data` the correct name?
2. **GitHub organization**: Should this be under a personal account or organization?
3. **License choice**: CC0 (public domain) or MIT for data files?
4. **Initial scope**: Start with 300-500 items or aim higher?
5. **Priority categories**: Which categories are most important for v1.0.0?
6. **Scraping approach**: Automated scraping vs. manual data entry for first version?
7. **Update frequency**: Quarterly updates acceptable or prefer different cadence?

**Recommended starting point:**
- Begin with **Phase 1** (repository setup)
- Then **Phase 2.1** (Goodwill scraper) as proof of concept
- Validate approach before scaling to all sources

This plan is designed to be iterative - we can adjust based on what works and what doesn't as we progress.
