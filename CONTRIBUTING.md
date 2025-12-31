# Contributing to myWriteOffs-data

Thank you for your interest in contributing to myWriteOffs-data! This repository provides public valuation data for charitable donations, and we welcome contributions from the community.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Reporting Data Errors](#reporting-data-errors)
- [Suggesting New Items](#suggesting-new-items)
- [Updating Valuations](#updating-valuations)
- [Development Process](#development-process)
- [Data Standards](#data-standards)
- [Pull Request Process](#pull-request-process)

## Code of Conduct

This project is committed to providing a welcoming and inclusive environment. Please be respectful and constructive in all interactions.

## How Can I Contribute?

### Reporting Data Errors

Found an incorrect valuation or typo? Please [open an issue](https://github.com/yourorg/myWriteOffs-data/issues/new?template=data-error.md) with:

- Item name and category
- Current (incorrect) value
- Correct value with source
- Link to public source (Goodwill, Salvation Army, IRS)

### Suggesting New Items

Want to add items not currently in the database? [Open an issue](https://github.com/yourorg/myWriteOffs-data/issues/new?template=new-item.md) with:

- Item name and description
- Suggested category
- Valuation with source
- Why this item is commonly donated

### Updating Valuations

Sources update their guides periodically. If you notice outdated valuations:

1. Check the source's current guide
2. [Open an issue](https://github.com/yourorg/myWriteOffs-data/issues/new?template=valuation-update.md) with the updated information
3. Include the source URL and date accessed

## Development Process

### Prerequisites

- Python 3.8 or higher
- Git
- Text editor or IDE

### Setting Up Your Development Environment

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/yourusername/myWriteOffs-data.git
   cd myWriteOffs-data
   ```

3. Create a virtual environment:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

4. Install dependencies:
   ```bash
   pip install -r scripts/requirements.txt
   ```

### Making Changes

1. Create a new branch:
   ```bash
   git checkout -b fix/item-name-correction
   ```

2. Make your changes to the JSON files in `data/`

3. Run validation scripts:
   ```bash
   python scripts/validate-data.py --all
   ```

4. Commit your changes:
   ```bash
   git add .
   git commit -m "Fix: Correct valuation for Men's Suit"
   ```

5. Push to your fork:
   ```bash
   git push origin fix/item-name-correction
   ```

## Data Standards

### JSON Format

All data files must be valid JSON and follow our schemas:

- Use 2-space indentation
- Include all required fields
- Provide source attribution
- Use ISO 8601 date formats (YYYY-MM-DD)

### Valuation Guidelines

- All valuations must come from public sources
- Include source ID and notes for each valuation
- Use decimal format for currency (e.g., 25.00, not 25)
- Recommended values should be average or median of sources

### Category Naming

- Use lowercase with hyphens (e.g., `mens-clothing`)
- Keep names concise but descriptive
- Follow existing category structure when possible

### Item Descriptions

- Be specific (e.g., "Men's Wool Suit" not "Suit")
- Include relevant details (material, condition, etc.)
- Keep descriptions under 200 characters
- Use title case for item names

## Pull Request Process

### Before Submitting

- [ ] Run validation scripts and ensure they pass
- [ ] Update CHANGELOG.md with your changes
- [ ] Ensure all JSON files are properly formatted
- [ ] Verify source attribution is included
- [ ] Test that files can be parsed correctly

### PR Requirements

1. **Title**: Use conventional commit format
   - `Fix: Correct valuation for item X`
   - `Add: New category for Y`
   - `Update: Refresh Goodwill valuations`

2. **Description**: Include:
   - What changed and why
   - Source links for new/updated data
   - Any breaking changes
   - Related issue numbers

3. **Validation**: PRs must pass automated validation checks

### Review Process

1. Maintainers will review your PR within 7 days
2. Address any requested changes
3. Once approved, maintainers will merge your PR
4. Your contribution will be included in the next release

## Data Sources

Only use data from these approved public sources:

- **Goodwill Valuation Guide**: https://www.goodwill.org/donate/donation-value-guide/
- **Salvation Army Valuation Guide**: https://satruck.org/Home/DonationValueGuide
- **IRS Publication 561**: https://www.irs.gov/pub/irs-pdf/p561.pdf

**Do not use**:
- Proprietary valuation services
- Intuit's ItsDeductible data
- Copyrighted materials
- Personal estimates without source backing

## Legal Compliance

By contributing, you agree that:

- Your contributions are your own work or properly attributed
- You have the right to submit the contribution
- Your contribution is licensed under CC-BY 4.0
- You have not included any proprietary or copyrighted data

## Questions?

- Check the [Project Plan](docs/Project-Plan.md) for architecture details
- Review existing [issues](https://github.com/yourorg/myWriteOffs-data/issues)
- Open a [discussion](https://github.com/yourorg/myWriteOffs-data/discussions) for questions

## Recognition

Contributors will be recognized in:
- CHANGELOG.md for each release
- GitHub's contributor graph
- Release notes

Thank you for helping make donation tracking more accessible!
