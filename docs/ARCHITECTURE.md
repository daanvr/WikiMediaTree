# Architecture Documentation

## System Overview
WikiMediaTree is a client-side hierarchical visualization application built around an interactive canvas system supporting user-driven exploration of Wikimedia hierarchies. The architecture centers on a block-based approach where each block represents Commons categories, Wikidata items, or both, connected in family tree-like structures. **Critical design principle**: The tool does NOT automatically generate trees because hierarchies have multiple parents and children per category/item, making automatic trees overwhelming with hundreds of interwoven nodes. **Primary focus**: Exploration and navigation interface with integration to existing Wikimedia editing workflows.

## Architecture Principles
- Client-side only application
- **Exploration-first approach** with external editing integration
- User-driven exploration (no automatic tree generation due to overwhelming hierarchy complexity)
- Modular component design
- Separation of concerns
- Modern JavaScript patterns
- Canvas-based visualization with smooth navigation (never empty canvas - always visible blocks)
- Block-centric data model with flexible Commons-Wikidata relationships
- **Ecosystem toggle** capability for focused exploration
- **API status transparency** with caching and loading state indicators
- Support for Commons transition to structured data through exploration insights

## Core Concepts

### Block System
Blocks are the fundamental building units of WikiMediaTree:
- **Commons Block**: Represents a Wikimedia Commons category
- **Wikidata Block**: Represents a Wikidata item
- **Hybrid Block**: Represents both a Commons category and corresponding Wikidata item
- **Parent-Child Relationships**: Each block (except root) has a parent and may have children

### Canvas Architecture
- **Interactive Canvas**: Pannable in all directions using Canvas API or SVG (**bounded to never show empty space**)
- **Viewport Management**: Handles visible area and coordinate transformations, ensures blocks always visible
- **Zoom/Pan Controls**: Smooth navigation across large hierarchies (hundreds of interwoven nodes)
- **Dynamic Navigation**: User-controlled hierarchy exploration with focus changes
- **Hierarchy Transitions**: Clicking navigation elements causes hierarchy to move, new blocks appear, old hierarchy disappears
- **Visual Indicators**: Small top left lines extending upward and fading, hover tooltips, expandable arrows
- **Ecosystem Toggle**: Filter and switch between Commons-only, Wikidata-only, or combined views
- **API Status Display**: Visual indicators for loading, caching, and network activity states

## System Components

### Core Modules

#### 1. Canvas Engine (`src/canvas/`)
- **CanvasManager**: Main canvas controller and viewport management
- **Renderer**: Handles block rendering and visual updates
- **EventHandler**: Manages user interactions (pan, zoom, click, hover)
- **CoordinateSystem**: Handles coordinate transformations and positioning

#### 2. Block System (`src/blocks/`)
- **Block**: Core block class with Commons/Wikidata properties
- **BlockManager**: Manages block lifecycle and relationships
- **BlockRenderer**: Handles individual block visualization
- **BlockFactory**: Creates blocks from API data

#### 3. Hierarchy Management (`src/hierarchy/`)
- **HierarchyController**: Manages tree structure and navigation
- **TreeBuilder**: Constructs tree structures from API data
- **NavigationManager**: Handles hierarchy traversal and focus changes

#### 4. API Layer (`src/api/`)
- **CommonsAPI**: Wikimedia Commons API interface
- **WikidataAPI**: Wikidata Query Service interface
- **DataMapper**: Maps and correlates Commons and Wikidata entities

#### 5. UI Components (`src/ui/`)
- **SitePanel**: Expandable site panel for detailed Wikidata item information
- **BlockDetails**: Expandable block information display with file/subcategory counts
- **VisualIndicators**: Small top left lines extending upward and fading, hover tooltips, and expandable arrows
- **NavigationControls**: Canvas navigation and hierarchy transition controls  
- **EcosystemToggle**: Toggle between Commons-only, Wikidata-only, or combined views
- **APIStatusIndicator**: Loading states, cached data indicators, and network activity feedback
- **ExternalLinkManager**: Direct links to Commons/Wikidata pages and creation workflows
- **StatusIndicators**: Visual indicators for block types, parent relationships, and missing entities

### Data Flow

1. **Initial Load** (User-Driven)
   ```
   User Search/Input → API Layer → Data Mapper → Block Factory → Hierarchy Controller → Canvas Renderer
   ```

2. **Hierarchy Navigation** (Lines Click)
   ```
   Line Click → Event Handler → Hierarchy Transition → Old Hierarchy Removal → New Block Loading → Canvas Update
   ```

3. **Block Expansion**
   ```
   Arrow Click → Block Manager → API Layer → Tree Builder → Hierarchy Extension → Canvas Renderer
   ```

4. **Hierarchy Switching**
   ```
   Switch Trigger → Hierarchy Controller → View State Change → Block Re-rendering → Transition Animation
   ```

5. **Site Panel Display**
   ```
   Wikidata Block → Site Panel Trigger → SPARQL Query → Data Formatting → Panel Display
   ```

6. **Ecosystem Toggle**
   ```
   Toggle Click → Ecosystem Filter → Block Re-rendering → Connection Updates → Canvas Refresh
   ```

7. **External Editing Integration**
   ```
   Block Link Click → URL Generation → External Navigation → (User Returns) → Context Preservation
   ```

8. **API Status Updates**
   ```
   API Call Start → Status Indicator Update → Request Processing → Response/Error → Status Update → Cache Management
   ```

### Component Structure
```
WikiMediaTree/
├── src/
│   ├── canvas/
│   │   ├── CanvasManager.js
│   │   ├── Renderer.js
│   │   ├── EventHandler.js
│   │   └── CoordinateSystem.js
│   ├── blocks/
│   │   ├── Block.js
│   │   ├── BlockManager.js
│   │   ├── BlockRenderer.js
│   │   └── BlockFactory.js
│   ├── hierarchy/
│   │   ├── HierarchyController.js
│   │   ├── TreeBuilder.js
│   │   └── NavigationManager.js
│   ├── api/
│   │   ├── CommonsAPI.js
│   │   ├── WikidataAPI.js
│   │   └── DataMapper.js
│   ├── ui/
│   │   ├── SitePanel.js
│   │   ├── BlockDetails.js
│   │   ├── VisualIndicators.js
│   │   ├── NavigationControls.js
│   │   ├── EcosystemToggle.js
│   │   ├── APIStatusIndicator.js
│   │   ├── ExternalLinkManager.js
│   │   └── StatusIndicators.js
│   ├── utils/
│   │   ├── Logger.js
│   │   ├── Cache.js
│   │   └── Helpers.js
│   ├── styles/
│   │   ├── canvas.css
│   │   ├── blocks.css
│   │   ├── ui.css
│   │   └── main.css
│   └── main.js
├── index.html
└── docs/
```

## Technology Decisions

### Frontend Architecture
- **Pure JavaScript (ES6+)**: No frameworks, direct DOM manipulation
- **Canvas/SVG Rendering**: For smooth visualization performance
- **Modular ES6 Imports**: Clean module separation
- **CSS Grid/Flexbox**: For UI layout components

### Visualization Technology
- **Canvas API**: Primary choice for smooth panning and rendering
- **SVG Fallback**: Alternative for accessibility and simpler interactions
- **CSS Transforms**: For smooth animations and transitions

### State Management
- **Event-Driven Architecture**: Component communication via custom events
- **Local State**: Each component manages its own state
- **Cache Layer**: API response caching for performance

### Design Patterns
- **Module Pattern**: Encapsulated functionality
- **Observer Pattern**: Event-driven state updates
- **Factory Pattern**: Block creation from diverse data sources
- **Strategy Pattern**: Different rendering strategies for block types

## Data Model

### Block Structure
```javascript
{
  id: "unique-identifier",
  type: "commons|wikidata|hybrid",
  commonsData: {
    title: "Category:Example",
    fileCount: 42,              // Files in Commons category
    subcategoryCount: 5,        // Subcategories (as shown on Commons)
    url: "https://commons.wikimedia.org/wiki/Category:Example"
  },
  wikidataData: {
    id: "Q12345",
    label: "Example",
    description: "An example item",
    instanceOf: "Q54321",       // P31 property
    image: "Example.jpg",       // P18 property  
    url: "https://www.wikidata.org/wiki/Q12345",
    otherProperties: {}         // Additional relevant properties
  },
  relationships: {
    parentId: "parent-block-id",
    childrenIds: ["child1-id", "child2-id"],
    parentType: "commons|wikidata|both",  // How parent is connected
    additionalParents: [                   // For small top left lines
      { id: "parent2-id", name: "Category:Parent2", type: "commons" },
      { id: "parent3-id", name: "Category:Parent3", type: "wikidata" }
    ]
  },
  visual: {
    position: { x: 100, y: 200 },
    expanded: false,
    showSitePanel: false,
    hasChildren: true,          // Show expandable arrow
    canSwitchHierarchy: true,   // Different connection types available
    ecosystemVisibility: {     // For ecosystem toggle
      commons: true,
      wikidata: true,
      currentFocus: "combined"  // "commons"|"wikidata"|"combined"
    },
    loadingState: "loaded",     // "loading"|"loaded"|"error"|"cached"
    missingEntities: {          // For missing entity indication
      needsCommonsCategory: false,
      needsWikidataItem: false,
      creationLinks: {
        commonsCreate: "url-to-creation-page",
        wikidataCreate: "url-to-creation-page"
      }
    }
  }
}
```

## Performance Considerations
- **Lazy Loading**: Load hierarchy data on demand
- **Viewport Culling**: Only render visible blocks
- **API Caching**: Cache Commons and Wikidata responses
- **Efficient Rendering**: Use requestAnimationFrame for smooth updates
- **Memory Management**: Cleanup unused blocks and event listeners

## Security Considerations
- **CORS Handling**: Proper configuration for external API calls
- **Input Sanitization**: Sanitize all user inputs and API responses
- **XSS Prevention**: Escape HTML content from external sources
- **API Rate Limiting**: Implement request throttling
- **No Authentication**: Read-only public API access only

## Scalability Architecture
- **Hierarchical Loading**: Progressive disclosure of tree branches
- **Chunked Rendering**: Render blocks in batches for large hierarchies
- **Background Processing**: Non-blocking API calls and data processing
- **Optimistic Updates**: Update UI immediately, sync with APIs asynchronously

---
*This document should be updated as the architecture evolves*