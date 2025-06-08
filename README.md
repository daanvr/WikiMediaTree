# WikiMediaTree

A hierarchical visualization tool for exploring the relationships between Wikimedia Commons categories and Wikidata items. WikiMediaTree provides an interactive canvas for understanding, navigating, and managing the complex hierarchies within the Wikimedia ecosystem.

## Overview

WikiMediaTree helps Wikimedians, developers, and content creators **explore and navigate** the intricate connections between Commons categories and Wikidata items through **user-driven exploration**. Built around a flexible block-based system, it enables manual discovery of hierarchical structures that are currently difficult to understand due to the chaotic and interwoven nature of Wikimedia hierarchies. The tool serves as an **exploration and navigation interface** that integrates with existing Wikimedia editing workflows.

**Critical Design Principle**: The tool does NOT automatically create or display entire trees because hierarchies typically have multiple parents and children per category/item, making automatic trees overwhelming with hundreds of interwoven nodes. Instead, it provides user-controlled exploration tools for step-by-step discovery and understanding.

### Key Features

- **User-Driven Canvas**: Pan freely in all directions with manual hierarchy exploration (never shows empty canvas - always keeps blocks visible)
- **Detailed Block Visualization**: Visual blocks showing file counts, subcategory counts, and connection indicators exactly as shown on Commons
- **Specific Navigation Elements**: Small top left lines extending upward and fading, showing additional parents with hover tooltips and click navigation that replaces entire hierarchy view  
- **Ecosystem Toggle**: Switch between Commons-only, Wikidata-only, or combined view to focus exploration on specific ecosystems
- **Exploration-First Approach**: Primary focus on navigation and understanding hierarchies, with direct links to existing Wikimedia interfaces for editing
- **API Status Indicators**: Clear visual feedback about loading states, cached data, and API activity
- **Flexible Relationship Handling**: Support categories without items, items without categories, and identify missing connections with links to creation pages
- **Wikidata Site Panel**: Expandable panel showing label, description, instance of, image, and other relevant properties
- **Hierarchy Problem Solving**: Address current pain points where users get lost in subcategories or can't see the complete hierarchical picture

## Target Users

**Primary Users**: Manual editors who need overview of hierarchical structure. These users may be technical but focus primarily on data contribution and content organization.

- **Commons Contributors**: Understanding category structures and finding appropriate placement for media files
- **Wikidata Editors**: Navigating complex property-based hierarchies and understanding item relationships  
- **Content Organizers**: Getting overview of chaotic category systems to make informed organizational decisions
- **Hierarchy Researchers**: Exploring where Commons categories and Wikidata items don't align for improvement opportunities
- **General Wikimedians**: Understanding the complete hierarchical picture that's currently fragmented across different interfaces

## Core Concepts

### Terminology

- **Items**: Always refers to Wikidata items (e.g., Q12345)
- **Categories**: Always refers to Wikimedia Commons categories (e.g., Category:Example)  
- **Blocks**: Building blocks representing either a Commons category, Wikidata item, or both
- **Canvas**: The pannable, interactive visualization area
- **Files**: Media files stored in Commons categories (images, videos, audio, etc.)
- **Site Panel**: Expandable panel showing detailed Wikidata item information

### Block System (Built on Flexible Assumptions)

WikiMediaTree operates on the **flexible assumption** that most Commons categories have Wikidata item equivalents, but this is **not one-to-one true**:

- **Commons Block**: Represents a Commons category (may or may not have corresponding Wikidata item)
- **Wikidata Block**: Represents a Wikidata item (may or may not have corresponding Commons category)
- **Hybrid Block**: Represents both a Commons category and corresponding Wikidata item when both exist
- **Parent-Child Relationships**: Each block (except root) has a parent and potential children, like in a family tree

Each block displays (exactly as shown on Commons):
- **File count**: Number of files in Commons category (prominently displayed)
- **Subcategory count**: Number of subcategories (prominently displayed)
- **Connection status**: Clear indicators showing Commons category presence, Wikidata item presence, or both
- **Parent relationship type**: How the parent is connected (via Commons, Wikidata, or both)
- **Direct links**: Clickable links to respective Commons and Wikidata pages
- **Visual elements**: Top left lines for additional parents, expandable arrows for children
- **Expandability**: Blocks default to reduced size but can be expanded with button

## Getting Started

### Prerequisites

- Modern desktop browser with ES6+ support
- Internet connection for API access
- No installation required (client-side only)

### Quick Start

**Typical User Session**: Search → Explore → Navigate to External Editing

1. Clone this repository
2. Open `index.html` in your browser
3. **Search for a specific category or Wikidata item** to begin exploration
4. **Explore the hierarchy** by clicking on subcategories, items, and sub-blocks
5. **Pan freely** across the canvas (never shows empty space - always keeps blocks visible)
6. **Use ecosystem toggle** to focus on Commons-only, Wikidata-only, or combined view
7. **Hover over small top left lines** to see additional parent category names
8. **Click on lines** to navigate - hierarchy moves, new blocks appear, old hierarchy disappears  
9. **Click expandable arrows** (pointing down) to load child blocks
10. **Expand blocks** with buttons to see detailed file counts and metadata
11. **Use site panel** when Wikidata items exist to see comprehensive properties
12. **Click direct links** to visit Commons categories or Wikidata items for actual editing
13. **Monitor API status indicator** for loading/caching information

**Remember**: This tool is for exploration and navigation - editing happens in existing Wikimedia interfaces!

## Documentation

Comprehensive documentation is available in the `docs/` directory:

- **[Requirements](docs/REQUIREMENTS.md)**: Functional and technical requirements
- **[Architecture](docs/ARCHITECTURE.md)**: System design and technical architecture
- **[Features](docs/FEATURES.md)**: Detailed feature specifications and user stories  
- **[API Endpoints](docs/API_ENDPOINTS.md)**: External API documentation and usage

## Technology Stack

- **Frontend**: Pure JavaScript (ES6+), HTML5, CSS3
- **Visualization**: Canvas API with SVG fallback
- **APIs**: Wikimedia Commons API, Wikidata Query Service (SPARQL)
- **Architecture**: Client-side only, no backend required

## Project Structure

```
WikiMediaTree/
├── src/                    # Source code
│   ├── canvas/            # Canvas engine and rendering
│   ├── blocks/            # Block system and management
│   ├── hierarchy/         # Tree structure and navigation
│   ├── api/               # External API interfaces
│   ├── ui/                # User interface components
│   └── utils/             # Utility functions
├── docs/                  # Documentation
├── index.html             # Main application entry point
└── README.md             # This file
```

## Contributing

We welcome contributions! Please:

1. Read the documentation in `docs/` to understand the system
2. Follow the code conventions outlined in `CLAUDE.md`
3. Test your changes across different browsers
4. Submit pull requests with clear descriptions

### Development Workflow

- Edit code only in the `src/` directory
- Follow modern JavaScript (ES6+) patterns
- Use semantic HTML and BEM CSS naming
- Document complex logic with clear comments
- Test all functionality before submitting

## Roadmap

### MVP Features (Phase 1) - Exploration Focus
- Interactive canvas with pan/zoom (bounded to always show blocks)
- Block visualization with connection status indicators
- User-driven hierarchical tree navigation
- Ecosystem toggle (Commons/Wikidata/combined views)
- Small top left lines for additional parent navigation
- API status indicators and caching
- Wikidata site panel with basic properties
- Direct links to existing Wikimedia editing interfaces

### Future Enhancements (Phase 2+) - Editing Integration
- Research and implement key Wikidata hierarchical properties
- Missing entity creation workflow integration  
- Advanced hierarchy manipulation (requires authentication)
- File organization within categories (needs UI/UX research)
- Enhanced visual hierarchy problem detection
- Performance optimization for large hierarchies

## API Integration

WikiMediaTree integrates with several Wikimedia APIs:

- **Commons API**: Category information, file counts, hierarchical relationships
- **Wikidata Query Service**: Item properties, SPARQL queries, hierarchical data
- **MediaWiki API**: General page information and metadata

See [API_ENDPOINTS.md](docs/API_ENDPOINTS.md) for detailed integration information.

## Browser Support

- Chrome/Chromium 70+
- Firefox 65+
- Safari 13+
- Edge 79+

*Mobile browsers are not supported - this is a desktop-only application.*

## Error Handling

WikiMediaTree gracefully handles various error scenarios:

- **API Failures**: Network issues or API downtime display user-friendly error messages
- **Missing Data**: Categories without items or items without categories are clearly indicated
- **Malformed Responses**: Invalid API responses trigger fallback behaviors
- **Rate Limiting**: Automatic retry logic with exponential backoff
- **Empty Hierarchies**: Clear messaging when no content is available

## Problem Statement

WikiMediaTree addresses specific pain points in current Wikimedia workflows:

### Commons Navigation Issues
- **Limited hierarchical visibility**: Commons shows subcategories and parent categories only as lists
- **Navigation confusion**: Users get lost in subcategories without knowing where they came from or how to go back  
- **Fragmented overview**: Impossible to see the complete hierarchical picture
- **Chaotic organization**: Categories are inconsistent, based on year, user uploads, or arbitrary organizational schemes

### Wikidata Complexity  
- **Multiple hierarchical properties**: Subclass (P279), located in administrative territory (P131), and many other properties create complex hierarchical relationships
- **Property navigation difficulty**: Very complicated to find correct parent and child items
- **Scattered hierarchy definition**: Different properties define different types of hierarchical relationships

### Structural Problems
- **Interwoven hierarchies**: Single files can end up in vastly different category paths when traversing upward
- **Multiple parents/children**: Each category typically has multiple parents and children, making visualization complex
- **Historical inconsistency**: Category systems grew organically over time with different organizational philosophies

WikiMediaTree provides a unified exploration interface to understand these complex, chaotic, and interwoven hierarchies.

## License

[Add your chosen license here]

## Acknowledgments

- Wikimedia Foundation for providing public APIs
- The Wikimedia community for creating the hierarchical structures we visualize
- Contributors and testers who help improve the tool

---

For more information, visit our [documentation](docs/) or contact the development team.
