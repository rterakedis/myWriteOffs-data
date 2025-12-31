# myWriteOffs-data

Public valuation data repository for the [myWriteOffs](https://github.com/yourorg/myWriteOffs) application.

## Overview

This repository hosts version-controlled donation item valuations compiled from public IRS-recommended sources. The data enables the myWriteOffs app to provide users with fair market value estimates for charitable donations without requiring proprietary research or backend infrastructure.

## Data Sources

All valuations are sourced from publicly available guides:

- **Goodwill Valuation Guide** - https://www.goodwill.org/donate/donation-value-guide/
- **Salvation Army Valuation Guide** - https://satruck.org/Home/DonationValueGuide
- **IRS Publication 561** - https://www.irs.gov/pub/irs-pdf/p561.pdf

## Repository Structure

```
myWriteOffs-data/
├── data/                    # JSON data files
│   ├── version.json        # Current version metadata
│   ├── categories/         # Category definitions
│   ├── items/              # Item valuations by category
│   └── metadata/           # Source attribution and standards
├── scripts/                # Data collection and validation scripts
├── archive/                # Historical data versions
├── tests/                  # Validation and quality tests
└── docs/                   # Documentation
```

## Data Architecture

The repository uses a **3-layer data model**:

1. **Base Layer** (GitHub) - Public valuation data from official sources
2. **Cache Layer** (App) - Downloaded data with sync metadata
3. **User Layer** (App) - Custom items and valuation overrides

This architecture ensures:
- User customizations are never lost during updates
- App works fully offline with cached data
- Regular updates without requiring app releases
- Full transparency of data sources

## Usage

### For App Developers

Download the latest data version:

```swift
let versionURL = "https://raw.githubusercontent.com/yourorg/myWriteOffs-data/main/data/version.json"
// Parse version.json to get data file URLs
// Download and cache category and item files
```

See [docs/Project-Plan.md](docs/Project-Plan.md) for detailed integration instructions.

### For Contributors

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Reporting data errors
- Suggesting new items or categories
- Updating valuations from public sources

## Data Updates

- **Frequency**: Quarterly (January, April, July, October) as a best effort
- **Process**: Automated scraping → Validation → Review → Release
- **Versioning**: Semantic versioning (v1.0.0, v1.1.0, etc.)

## Legal & Compliance

- All data sourced from public guides with full attribution
- No proprietary research or algorithms
- Designed to avoid patent infringement
- See [LICENSE](LICENSE) for data usage terms

## Current Status

**Version**: Pre-release (v0.0.0)  
**Items**: 0  
**Categories**: 0  
**Last Updated**: Not yet released

## Links

- **Main App Repository**: [myWriteOffs](https://github.com/yourorg/myWriteOffs)
- **Project Plan**: [docs/Project-Plan.md](docs/Project-Plan.md)
- **Data Architecture**: [docs/github-data-architecture.md](docs/github-data-architecture.md) (in main app repo)
- **Issues**: [Report data errors or suggestions](https://github.com/yourorg/myWriteOffs-data/issues)

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history and updates.
