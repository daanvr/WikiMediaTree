# Requirements Specification

## Overview
WikiMediaTree is a hierarchical visualization tool for exploring the relationships between Wikimedia Commons categories and Wikidata items. It provides an interactive canvas for understanding, navigating, and managing the complex hierarchies within the Wikimedia ecosystem.

## Terminology
- **Items**: Always refers to Wikidata items (e.g., Q12345)
- **Categories**: Always refers to Wikimedia Commons categories (e.g., Category:Example) 
- **Blocks**: Building blocks representing either a Commons category, Wikidata item, or both
- **Canvas**: The pannable, interactive visualization area
- **Files**: Media files stored in Commons categories (images, videos, audio, etc.)
- **Site Panel**: Expandable panel showing detailed Wikidata item information

## Functional Requirements

### Core Functionality
1. **User-Driven Hierarchical Visualization**
   - Display hierarchies as family tree-like structures on an interactive canvas
   - Support panning in all directions across the canvas
   - Show parent-child relationships between blocks
   - Allow manual tree growth in any direction based on user exploration
   - **Critical**: Tool does NOT automatically create trees (real hierarchies are chaotic)
   - User controls which parts of the hierarchy to explore and display

2. **Block Management**
   - Each block represents a Commons category, Wikidata item, or both
   - Display block connection status indicators (Commons, Wikidata, or hybrid)
   - Show parent relationship type explicitly (connected via Commons, Wikidata, or both)
   - Display file count (media files) from Commons categories
   - Display subcategory count from Commons categories  
   - Allow removing/quitting parts of the tree from current view
   - Provide direct links to respective Commons and Wikidata pages

3. **Flexible Data Integration**
   - Built on flexible assumption that most Commons categories have Wikidata equivalents
   - **Not one-to-one mapping**: Handle categories without corresponding items
   - Handle Wikidata items without corresponding categories
   - Clearly indicate when both category and item exist for the same concept
   - Support exploration of hierarchy mismatches between Commons and Wikidata

### User Requirements
1. **Target Users**
   - Developers in the Wikimedia ecosystem
   - Content creators and editors
   - General Wikimedians
   - Commons and Wikidata editors

2. **User Goals**
   - Understand complex hierarchical relationships in Wikimedia ecosystem
   - Make edits by moving things around in hierarchies
   - Sort files within hierarchies using drag and drop
   - Explore and identify hierarchy mismatches between Commons and Wikidata
   - Clean and organize hierarchical structures
   - **Support transition**: Help Wikimedia Commons transition from traditional category-based organization to structured data approach
   - Create and enrich missing corresponding categories or items where applicable
   - Generally explore, understand, and improve hierarchical organization

### Block Features
1. **Display Information**
   - File count (number of files in Commons category) - displayed prominently
   - Subcategory count - displayed prominently (as shown on Commons)
   - Connection status indicators (Commons category, Wikidata item, or both)
   - Parent relationship type indicators (connected via Commons, Wikidata, or both)
   - Direct clickable links to respective Commons and Wikidata pages
   - Blocks default to reduced size but expandable with button

2. **Specific Visual Elements**
   - **Top left lines**: Visual lines positioned on top left of blocks, extending upward and fading, indicating additional parent categories not currently displayed
   - **Hover tooltips**: Category names appear when hovering over the top left lines
   - **Click navigation**: Clicking lines makes hierarchy move, new blocks appear, old hierarchy disappears
   - **Expandable arrows**: Small downward-pointing arrows indicating available children, click to load and display child blocks in hierarchy
   - **Hierarchy switching**: When parent lines show different connection types, allow switching between Commons and Wikidata hierarchy views

3. **Interaction Behaviors**
   - Blocks can be expanded via dedicated buttons to show detailed information (full file counts, category descriptions, additional metadata)
   - Drag and drop functionality specifically for sorting files within hierarchies
   - Different visual states for Commons-only, Wikidata-only, or hybrid blocks
   - Context-sensitive actions based on block type

### Wikidata Integration
1. **Site Panel** (Expandable Side Panel)
   - **Trigger**: When a Wikidata item exists for a block, make expandable site panel available
   - **Basic Information Display**: 
     - Label (item name)
     - Description 
     - Instance of (P31)
     - Associated image (P18)
     - Other relevant Wikidata properties and values
   - **Panel Behavior**: Can be expanded/collapsed, shows comprehensive item details
   - **Direct linking**: Include link to full Wikidata item page

### Performance Requirements
- Smooth panning and zooming on canvas
- Responsive interaction with blocks and UI elements
- Efficient loading of hierarchy data
- Handle large hierarchical structures without performance degradation

### Security Requirements
- Read-only access to public Wikimedia APIs
- No authentication required for viewing
- Secure handling of external API requests

## Technical Requirements

### Browser Support
- Desktop browsers only (no mobile/responsive needed)
- Modern browsers supporting ES6+
- Canvas/SVG support for visualization

### Technology Stack
- Client-side only architecture
- Modern JavaScript (ES6+)
- HTML5/CSS3
- Canvas or SVG for visualization
- Fetch API for external requests

### API Integration
- Wikimedia Commons API
- Wikidata Query Service (SPARQL)
- MediaWiki API

### Constraints
- Client-side only (no backend server)
- Must handle CORS for external API calls
- Real-time hierarchy data fetching
- No user authentication system needed

## Acceptance Criteria
1. Users can visualize hierarchies as interactive family trees through user-driven exploration
2. Users can pan freely across the canvas in all directions with smooth performance
3. Blocks correctly display Commons category and Wikidata item status with clear indicators
4. **Top left lines** show additional parents with hover tooltips displaying category names
5. **Click navigation** on lines causes hierarchy to move, new blocks appear, old hierarchy disappears
6. Users can expand/collapse hierarchy branches using expandable arrows pointing down
7. **Site panel** displays comprehensive Wikidata information (label, description, instance of, image)
8. **Hierarchy switching** between Commons and Wikidata works when different connection types exist
9. File counts and subcategory counts are prominently displayed (as shown on Commons)
10. Direct links to Commons and Wikidata pages function correctly
11. **Drag and drop** functionality works specifically for sorting files within hierarchies
12. Tool serves as effective **transition aid** from category-based to structured data organization
13. Flexible handling of categories without items and items without categories
14. Visual identification of hierarchy mismatches between Commons and Wikidata
15. Ability to remove/quit parts of tree from current view

---
*This document should be updated as requirements evolve*