# WikiMediaTree

A hierarchical visualization tool for exploring the relationships between Wikimedia Commons categories and Wikidata items. WikiMediaTree provides an interactive canvas for understanding, navigating, and managing the complex hierarchies within the Wikimedia ecosystem.

## Overview

WikiMediaTree helps Wikimedians, developers, and content creators visualize and organize the intricate connections between Commons categories and Wikidata items through **user-driven exploration**. Built around a flexible block-based system, it enables manual discovery of hierarchical structures, identification of mismatches, and serves as a **transition tool** to help Wikimedia Commons move from traditional category-based organization to structured data approaches.

**Critical Design Principle**: The tool does NOT automatically create or display entire trees due to the chaotic nature of real hierarchies. Instead, it provides user-controlled exploration tools for step-by-step discovery and organization.

### Key Features

- **User-Driven Canvas**: Pan freely in all directions with manual hierarchy exploration (no automatic tree generation)
- **Detailed Block Visualization**: Visual blocks showing file counts, subcategory counts, and connection indicators exactly as shown on Commons
- **Specific Navigation Elements**: Top left lines showing additional parents with hover tooltips and click navigation that replaces entire hierarchy view  
- **Hierarchy Editing**: Make edits by moving things around, sort photos/files by drag and drop within hierarchies
- **Transition Tool Support**: Help Commons transition from category-based to structured data organization
- **Flexible Relationship Handling**: Support categories without items, items without categories, and identify missing connections
- **Wikidata Site Panel**: Expandable panel showing label, description, instance of, image, and other relevant properties
- **Mismatch Exploration**: Explore where Commons and Wikidata hierarchies don't align for creating missing entities

## Target Users

- **Wikimedia Developers**: Understanding and improving ecosystem architecture
- **Content Creators**: Organizing media and content within appropriate hierarchies
- **Wikidata Editors**: Managing item relationships and structured data
- **Commons Contributors**: Organizing categories and files
- **General Wikimedians**: Exploring and understanding project relationships

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

1. Clone this repository
2. Open `index.html` in your browser
3. **Search for a specific category or item** to begin exploration
4. **Pan freely** across the canvas in all directions using mouse/trackpad
5. **Hover over top left lines** to see additional parent category names
6. **Click on lines** to navigate - hierarchy will move, new blocks appear, old hierarchy disappears
7. **Click expandable arrows** (pointing down) to grow tree in that direction
8. **Expand blocks** with buttons to see detailed information
9. **Use site panel** when Wikidata items exist to see detailed properties
10. **Remove/quit parts** of the tree as needed to focus on specific areas

**Remember**: This is user-driven exploration - the tool doesn't automatically create trees!

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

### MVP Features (Phase 1)
- Interactive canvas with pan/zoom
- Basic block visualization
- Hierarchical tree navigation
- Visual hierarchy indicators
- Wikidata information panel

### Future Enhancements (Phase 2+)
- Advanced drag & drop functionality
- Hierarchy view switching
- Mismatch detection and highlighting
- Search and navigation tools
- Collaborative editing capabilities

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

## License

[Add your chosen license here]

## Acknowledgments

- Wikimedia Foundation for providing public APIs
- The Wikimedia community for creating the hierarchical structures we visualize
- Contributors and testers who help improve the tool

---

For more information, visit our [documentation](docs/) or contact the development team.
