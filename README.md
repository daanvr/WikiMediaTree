# WikiMediaTree

A hierarchical visualization tool for exploring the relationships between Wikimedia Commons categories and Wikidata items. WikiMediaTree provides an interactive canvas for understanding, navigating, and managing the complex hierarchies within the Wikimedia ecosystem.

## Overview

WikiMediaTree helps Wikimedians, developers, and content creators visualize and organize the intricate connections between Commons categories and Wikidata items. Built around a flexible block-based system, it enables users to explore hierarchical structures, identify mismatches, and support the transition from category-based to structured data organization.

### Key Features

- **Interactive Canvas**: Pan and zoom freely across large hierarchical structures
- **Block-Based Visualization**: Visual blocks representing Commons categories, Wikidata items, or both
- **Hierarchical Navigation**: Family tree-like structures with expandable branches
- **Dual Hierarchy Support**: Switch between Commons and Wikidata hierarchy views
- **Mismatch Detection**: Identify inconsistencies between Commons and Wikidata hierarchies
- **Drag & Drop Organization**: Reorganize content through intuitive interactions
- **Rich Information Panels**: Detailed Wikidata item information and metadata

## Target Users

- **Wikimedia Developers**: Understanding and improving ecosystem architecture
- **Content Creators**: Organizing media and content within appropriate hierarchies
- **Wikidata Editors**: Managing item relationships and structured data
- **Commons Contributors**: Organizing categories and files
- **General Wikimedians**: Exploring and understanding project relationships

## Core Concepts

### Terminology

- **Items**: Wikidata items (e.g., Q12345)
- **Categories**: Wikimedia Commons categories (e.g., Category:Example)
- **Blocks**: Visual building blocks representing categories, items, or both
- **Canvas**: The interactive visualization area supporting pan/zoom navigation

### Block System

WikiMediaTree uses blocks as fundamental building units:

- **Commons Block**: Represents a Wikimedia Commons category
- **Wikidata Block**: Represents a Wikidata item  
- **Hybrid Block**: Represents both a Commons category and corresponding Wikidata item
- **Parent-Child Relationships**: Hierarchical connections between blocks

Each block displays:
- Connection status (Commons, Wikidata, or both)
- File count and subcategory count
- Parent relationship indicators
- Direct links to respective pages

## Getting Started

### Prerequisites

- Modern desktop browser with ES6+ support
- Internet connection for API access
- No installation required (client-side only)

### Quick Start

1. Clone this repository
2. Open `index.html` in your browser
3. Start exploring by searching for a category or item
4. Use canvas controls to pan and zoom
5. Click blocks to expand hierarchies

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

## License

[Add your chosen license here]

## Acknowledgments

- Wikimedia Foundation for providing public APIs
- The Wikimedia community for creating the hierarchical structures we visualize
- Contributors and testers who help improve the tool

---

For more information, visit our [documentation](docs/) or contact the development team.
